---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, nginx, ingress controller

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


# Log de mudanças do Fluentd e do ALB do Ingress
{: #cluster-add-ons-changelog}

Seu cluster do {{site.data.keyword.containerlong}} é fornecido com componentes, como os componentes Fluentd e ALB do Ingress, que são atualizados automaticamente pela IBM. Também é possível desativar as atualizações automáticas para alguns componentes e atualizá-las manualmente separadamente dos nós principal e do trabalhador. Consulte as tabelas nas seções a seguir para obter um resumo das mudanças para cada versão.
{: shortdesc}

Para obter mais informações sobre como gerenciar atualizações para o Fluentd e os ALBs do Ingress, consulte [Atualizando componentes do cluster](/docs/containers?topic=containers-update#components).

## Log de mudanças de ALBs do Ingress
{: #alb_changelog}

Visualize as mudanças da versão de construção para os balanceadores de carga do aplicativo (ALBs) do Ingress em seus clusters do {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Quando o componente ALB do Ingress é atualizado, os contêineres `nginx-ingress` e `ingress-auth` em todos os pods do ALB são atualizados para a versão de construção mais recente. Por padrão, as atualizações automáticas para o componente são ativadas, mas é possível desativá-las e atualizar manualmente o componente. Para obter mais informações, consulte [Atualizando o balanceador de carga do aplicativo Ingress](/docs/containers?topic=containers-update#alb).

Consulte a tabela a seguir para obter um resumo das mudanças para cada construção do componente ALB do Ingress.

<table summary="Visão geral de mudanças de construção para o componente de balanceador de carga do aplicativo do Ingress">
<caption>Log de mudanças para o componente de balanceador de carga do aplicativo do Ingress</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>`nginx-ingress` / `ingress-auth` build</th>
<th>Data de liberação</th>
<th>Mudanças sem interrupções</th>
<th>Mudanças disruptivas</th>
</tr>
</thead>
<tbody>
<tr>
<td>470/330</td>
<td>07 de junho de 2019</td>
<td>Corrige as vulnerabilidades do BD Berkeley para [CVE-2019-8457 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>470/329</td>
<td>06 de junho de 2019</td>
<td>Corrige as vulnerabilidades do BD Berkeley para [CVE-2019-8457 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8457).
</td>
<td>-</td>
</tr>
<tr>
<td>467/329</td>
<td>03 de junho de 2019</td>
<td>Corrige vulnerabilidades do GnuTLS para [CVE-2019-3829 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3829), [CVE-2019-3836 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3836), [CVE-2019-3893 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-3893), [CVE-2018-10844 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10845), [CVE-2018-10845 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10844) e [CVE-2018-10846 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-10846).
</td>
<td>-</td>
</tr>
<tr>
<td>462/329</td>
<td>28 de maio de 2019</td>
<td>Corrige vulnerabilidades do cURL para [CVE-2019-5435 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5435) e [CVE-2019-5436 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-5436).</td>
<td>-</td>
</tr>
<tr>
<td>457/329</td>
<td>23 de maio de 2019</td>
<td>Corrige vulnerabilidades do Go para varreduras de imagem.</td>
<td>-</td>
</tr>
<tr>
<td>423/329</td>
<td>13 de maio de 2019</td>
<td>Melhora o desempenho para a integração com o {{site.data.keyword.appid_full}}.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 315</td>
<td>15 de abril de 2019</td>
<td>Atualiza o valor da expiração do cookie do {{site.data.keyword.appid_full_notm}} para que corresponda ao valor da expiração do token de acesso.</td>
<td>-</td>
</tr>
<tr>
<td>411 / 306</td>
<td>22 de março de 2019</td>
<td>Atualiza a versão do Go para 1.12.1.</td>
<td>-</td>
</tr>
<tr>
<td>410 / 305</td>
<td>18 de março de 2019</td>
<td><ul>
<li>Corrige vulnerabilidades para varreduras de imagem.</li>
<li>Melhora a criação de log para a integração com o {{site.data.keyword.appid_full_notm}}.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>408 / 304</td>
<td>05 de março de 2019</td>
<td>-</td>
<td>Corrige erros na integração de autorização que estão relacionados à funcionalidade de logout, expiração do token e o retorno de chamada de autorização `OAuth`. Essas correções serão implementadas apenas se você tiver ativado a autorização do {{site.data.keyword.appid_full_notm}} usando a anotação [`appid-auth`](/docs/containers?topic=containers-ingress_annotation#appid-auth). Para implementar essas correções, os cabeçalhos adicionais são incluídos, o que aumenta o tamanho total do cabeçalho. Dependendo do tamanho de seus próprios cabeçalhos e do tamanho total de respostas, pode ser necessário ajustar quaisquer [anotações de buffer do proxy](/docs/containers?topic=containers-ingress_annotation#proxy-buffer) que você usa.</td>
</tr>
<tr>
<td>406 / 301</td>
<td>19 de fevereiro de 2019</td>
<td><ul>
<li>Atualiza a versão do Go para 1.11.5.</li>
<li>Corrige vulnerabilidades para varreduras de imagem.</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>404 / 300</td>
<td>31 de janeiro de 2019</td>
<td>Atualiza a versão do Go para 1.11.4.</td>
<td>-</td>
</tr>
<tr>
<td>403 / 295</td>
<td>23 de janeiro de 2019</td>
<td><ul>
<li>Atualiza a versão NGINX de ALBs para 1.15.2.</li>
<li>Agora, os certificados TLS fornecidos pela IBM são renovados automaticamente 37 dias antes de expirarem em vez de 7 dias.</li>
<li>Inclui a funcionalidade de logout do {{site.data.keyword.appid_full_notm}}: se o prefixo `/logout` existir em um caminho do {{site.data.keyword.appid_full_notm}}, os cookies serão removidos e o usuário será enviado de volta para a página de login.</li>
<li>Inclui um cabeçalho nas solicitações do {{site.data.keyword.appid_full_notm}} para propósitos de rastreamento interno.</li>
<li>Atualiza a diretiva de local do {{site.data.keyword.appid_short_notm}} para que a anotação `app-id` possa ser usada com as anotações `proxy-buffers`, `proxy-buffer-size` e `proxy-busy-buffer-size`.</li>
<li>Corrige um erro para que os logs informativos não sejam rotulados como erros.</li>
</ul></td>
<td>Desativa o TLS 1.0 e 1.1 por padrão. Se os clientes que se conectarem a seus apps suportarem TLS 1.2, nenhuma ação será necessária. Se você ainda tiver clientes anteriores que requerem suporte do TLS 1.0 ou 1.1, ative manualmente as versões do TLS necessárias seguindo [estas etapas](/docs/containers?topic=containers-ingress#ssl_protocols_ciphers). Para obter mais informações sobre como ver as versões do TLS que seus clientes usam para acessar seus apps, consulte esta [Postagem do blog do {{site.data.keyword.Bluemix_notm}}](https://www.ibm.com/blogs/bluemix/2018/11/ibm-cloud-kubernetes-service-alb-update-tls-1-0-and-1-1-disabled-by-default/).</td>
</tr>
<tr>
<td>393 / 291</td>
<td>09 de janeiro de 2019</td>
<td>Inclui suporte para integração com múltiplas instâncias do {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>393 / 282</td>
<td>29 de novembro de 2018</td>
<td>Melhora o desempenho para a integração com o {{site.data.keyword.appid_full_notm}}.</td>
<td>-</td>
</tr>
<tr>
<td>384 / 246</td>
<td>14 de novembro de 2018</td>
<td>Melhora os recursos de criação de log e de logout para a integração com o {{site.data.keyword.appid_full_notm}}.</td>
<td>Substitui o certificado autoassinado para `*.containers.mybluemix.net` com o certificado assinado LetsEncrypt que é gerado automaticamente para o cluster e é usado por ele. O certificado autoassinado `*.containers.mybluemix.net` é removido.</td>
</tr>
<tr>
<td>350 / 192</td>
<td>05 de novembro de 2018</td>
<td>Inclui suporte para ativar e desativar atualizações automáticas do componente ALB do Ingress.</td>
<td>-</td>
</tr>
</tbody>
</table>

## Log de mudanças do Fluentd para criação de log
{: #fluentd_changelog}

Visualize as mudanças de versão da construção para o componente Fluentd para criação de log em seus clusters do {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

Por padrão, as atualizações automáticas para o componente são ativadas, mas é possível desativá-las e atualizar manualmente o componente. Para obter mais informações, consulte [Gerenciando atualizações automáticas para Fluentd](/docs/containers?topic=containers-update#logging-up).

Consulte a tabela a seguir para obter um resumo das mudanças para cada construção do componente Fluentd.

<table summary="Visão geral de mudanças de construção para o componente Fluentd">
<caption>Log de mudanças para o componente Fluentd</caption>
<col width="12%">
<col width="12%">
<col width="41%">
<col width="35%">
<thead>
<tr>
<th>Construção do Fluentd</th>
<th>Data de liberação</th>
<th>Mudanças sem interrupções</th>
<th>Mudanças disruptivas</th>
</tr>
</thead>
<tr>
<td>e7c10d74350dc64d4d92ba7f72bb4ff9219315d2</td>
<td>30 de maio de 2019</td>
<td>Atualiza o mapa de configuração do Fluent para sempre ignorar logs de pod de namespaces da IBM, mesmo quando o principal do Kubernetes está indisponível.</td>
<td>-</td>
</tr>
<tr>
<td>c16fe1602ab65db4af0a6ac008f99ca2a526e6f6</td>
<td>21 de maio de 2019</td>
<td>Corrige um erro no qual as métricas do nó do trabalhador não são exibidas.</td>
<td>-</td>
</tr>
<tr>
<td>60fc11f7bd39d9c6cfed923c598bf6457b3f2037</td>
<td>10 de maio de 2019</td>
<td>Atualiza os pacotes do Ruby para [CVE-2019-8320 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) e [CVE-2019-8325 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>91a737f68f7d9e81b5d2223c910aaa7d7f91b76d</td>
<td>08 de maio de 2019</td>
<td>Atualiza os pacotes do Ruby para [CVE-2019-8320 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8320), [CVE-2019-8321 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8321), [CVE-2019-8322 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8322), [CVE-2019-8323 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323), [CVE-2019-8324 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8324) e [CVE-2019-8325 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8325).</td>
<td>-</td>
</tr>
<tr>
<td>d9af69e286986a05ed4a50469585b1cf978ddb1d</td>
<td>11 de abril de 2019</td>
<td>Atualiza o plug-in do cAdvisor para usar o TLS 1.2.</td>
<td>-</td>
</tr>
<tr>
<td>3100ddb62580a9f46ffdff7bab2ebec40b164de6</td>
<td>01 de abril de 2019</td>
<td>Atualiza a conta do serviço Fluentd.</td>
<td>-</td>
</tr>
<tr>
<td>c85567b75bd7ad1c9428794cd63a8e239c3fd8f5</td>
<td>18 de março de 2019</td>
<td>Remove a dependência no cURL para [CVE-2019-8323 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2019-8323).</td>
<td>-</td>
</tr>
<tr>
<td>320ffdf87de068ee2f7f34c0e7a47a111e8d457b</td>
<td>18 de fevereiro de 2019</td>
<td><ul>
<li>Atualiza o Fluend para a versão 1.3.</li>
<li>Remove o Git da imagem do Fluentd para [CVE-2018-19486 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2018-19486).</li>
</ul></td>
<td>-</td>
</tr>
<tr>
<td>972865196aefd3324105087878de12c518ed579f</td>
<td>01 de janeiro de 2019</td>
<td><ul>
<li>Ativa a codificação UTF-8 para o plug-in `in_tail` do Fluentd.</li>
<li>Correções de erros menores.</li>
</ul></td>
<td>-</td>
</tr>
</tbody>
</table>
