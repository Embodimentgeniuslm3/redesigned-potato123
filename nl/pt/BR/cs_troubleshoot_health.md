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
{:download: .download}
{:preview: .preview}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}


# Resolução de problemas de criação de log e monitoramento
{: #cs_troubleshoot_health}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas com a criação de log e o monitoramento.
{: shortdesc}

Se você tiver um problema mais geral, tente a [depuração do cluster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

## Os logs não aparecem
{: #cs_no_logs}

{: tsSymptoms}
Quando você acessa o painel do Kibana, seus logs não são exibidos.

{: tsResolve}
Revise as razões a seguir para saber o motivo de os logs do cluster não aparecerem e as etapas de resolução de problemas correspondentes:

<table>
<caption>Resolução de logs que não exibem</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Por que isso está acontecendo?</th>
      <th>Como corrigi-lo</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Nenhuma configuração de criação de log está definida.</td>
    <td>Para que os logs sejam enviados, deve-se criar uma configuração de criação de log. Para fazer isso, veja <a href="/docs/containers?topic=containers-health#logging">Configurando a criação de log do cluster</a>.</td>
  </tr>
  <tr>
    <td>O cluster não está em um estado <code>Normal</code>.</td>
    <td>Para verificar o estado do seu cluster, veja <a href="/docs/containers?topic=containers-cs_troubleshoot#debug_clusters">Depurando clusters</a>.</td>
  </tr>
  <tr>
    <td>A cota de armazenamento de log foi atendida.</td>
    <td>Para aumentar seus limites de armazenamento de log, consulte a <a href="/docs/services/CloudLogAnalysis/troubleshooting?topic=cloudloganalysis-error_msgs">documentação do {{site.data.keyword.loganalysislong_notm}}</a>.</td>
  </tr>
  <tr>
    <td>Se você especificou um espaço na criação do cluster, o proprietário da conta não tem permissões de Gerenciador, Desenvolvedor ou Auditor para esse espaço.</td>
      <td>Para mudar permissões de acesso para o proprietário da conta:
      <ol><li>Para descobrir quem é o proprietário da conta para o cluster, execute <code>ibmcloud ks api-key-info --cluster &lt;cluster_name_or_ID&gt;</code>.</li>
      <li>Para conceder a esse proprietário da conta as permissões de acesso de Gerenciador, Desenvolvedor ou Auditor do {{site.data.keyword.containerlong_notm}} para o espaço, veja <a href="/docs/containers?topic=containers-users">Gerenciando o acesso ao cluster</a>.</li>
      <li>Para atualizar o token de criação de log após a mudança das permissões, execute <code>ibmcloud ks logging-config-refresh --cluster &lt;cluster_name_or_ID&gt;</code>.</li></ol></td>
    </tr>
    <tr>
      <td>Você tem uma configuração de criação de log para seu app com um symlink em seu caminho de app.</td>
      <td><p>Para que os logs sejam enviados, deve-se usar um caminho absoluto na configuração de criação de log ou os logs não poderão ser lidos. Se o seu caminho estiver montado para seu nó do trabalhador, ele poderá criar um symlink.</p> <p>Exemplo: se o caminho especificado é <code>/usr/local/<b>spark</b>/work/app-0546/0/stderr</code>, mas os logs vão para <code>/usr/local/<b>spark-1.0-hadoop-1.2</b>/work/app-0546/0/stderr</code>, os logs não podem ser lidos.</p></td>
    </tr>
  </tbody>
</table>

Para testar as mudanças feitas durante a resolução de problemas, é possível implementar *Noisy*, um pod de amostra que produz vários eventos de log, para um nó do trabalhador em seu cluster.

Antes de iniciar: [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1. Crie o arquivo de configuração `deploy-noisy.yaml`.
    ```
    apiVersion: v1
      kind: Pod
      metadata:
        name: noisy
      spec:
        containers:
      - name: noisy
          image: ubuntu:16.04
          command: ["/bin/sh"]
          args: ["-c", "while true; do sleep 10; echo 'Hello world!'; done"]
          imagePullPolicy: "Always"
      ```
      {: codeblock}

2. Execute o arquivo de configuração no contexto do cluster.
    ```
    Kubectl apply -f `deploy-noisy.yaml
    ```
    {:pre}

3. Após alguns minutos, é possível visualizar seus logs no painel do Kibana. Para acessar o painel do Kibana, acesse uma das URLs a seguir e selecione a conta do {{site.data.keyword.Bluemix_notm}} na qual você criou o cluster. Se você especificou um espaço na criação do cluster, acesse esse espaço.
    - US-Sul e EUA/Leste:  ` https://logging.ng.bluemix.net `
    - UK-Sul:  ` https://logging.eu-gb.bluemix.net `
    - UE-Central:  ` https://logging.eu-fra.bluemix.net `
    - AP-Sul: `https://logging.au-syd.bluemix.net`

<br />


## O painel do Kubernetes não exibe gráficos de utilização
{: #cs_dashboard_graphs}

{: tsSymptoms}
Quando você acessa o painel do Kubernetes, os gráficos de utilização não são exibidos.

{: tsCauses}
Às vezes, após uma atualização do cluster ou uma reinicialização do nó do trabalhador, o pod `kube-dashboard` não é atualizado.

{: tsResolve}
Exclua o pod `kube-painel` para forçar uma reinicialização. O pod é recriado com políticas RBAC para acessar `heapster` para obter informações de utilização.

  ```
  kubectl delete pod -n kube-system $(kubectl get pod -n kube-system --selector=k8s-app=kubernetes-dashboard -o jsonpath='{.items..metadata.name}')
  ```
  {: pre}

<br />


## A cota do log está muito baixa
{: #quota}

{: tsSymptoms}
Você define uma configuração de criação de log em seu cluster para encaminhar logs para o {{site.data.keyword.loganalysisfull}}. Ao visualizar os logs, você vê uma mensagem de erro semelhante à seguinte:

```
Você atingiu a cota diária que está alocada para o espaço do Bluemix {GUID do espaço} para a instância do IBM® Cloud Log Analysis {GUID da instância}. Sua dotação diária atual é XXX para armazenamento de Procura de log, que ficará retida por um período de 3 dias, durante o qual poderá ser procurada no Kibana. Isso não afeta a política de retenção de log no armazenamento de Coleção de logs. Para fazer upgrade de seu plano para que seja possível armazenar mais dados no armazenamento de procura de log por dia, faça upgrade do plano de serviço do Log Analysis para esse espaço. Para obter mais informações sobre planos de serviço e como fazer upgrade de seu plano, consulte Planos.
```
{: screen}

{: tsResolve}
Revise as razões a seguir por que você está atingindo sua cota de log e as etapas de resolução de problemas correspondentes:

<table>
<caption>Resolução de problemas de armazenamento de log</caption>
  <col width="40%">
  <col width="60%">
  <thead>
    <tr>
      <th>Por que isso está acontecendo?</th>
      <th>Como corrigi-lo</th>
    </tr>
 </thead>
 <tbody>
  <tr>
    <td>Um ou mais pods produzem um número alto de logs.</td>
    <td>É possível liberar espaço de armazenamento de log evitando que os logs de pods específicos sejam encaminhados. Crie um [filtro de criação de log](/docs/containers?topic=containers-health#filter-logs) para esses pods.</td>
  </tr>
  <tr>
    <td>Você excede a alocação diária de 500 MB para armazenamento de log para o plano Lite.</td>
    <td>Primeiro, [calcule a cota de procura e o uso diário](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) de seu domínio de logs. Em seguida, é possível aumentar sua cota de armazenamento de log [fazendo upgrade de seu plano de serviço do {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  <tr>
    <td>Você está excedendo a cota de armazenamento de log para o seu plano pago atual.</td>
    <td>Primeiro, [calcule a cota de procura e o uso diário](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-quota) de seu domínio de logs. Em seguida, é possível aumentar sua cota de armazenamento de log [fazendo upgrade de seu plano de serviço do {{site.data.keyword.loganalysisshort_notm}}](/docs/services/CloudLogAnalysis/how-to?topic=cloudloganalysis-change_plan#change_plan).</td>
  </tr>
  </tbody>
</table>

<br />


## As linhas de log são muito longas
{: #long_lines}

{: tsSymptoms}
Você define uma configuração de criação de log em seu cluster para encaminhar logs para o {{site.data.keyword.loganalysisfull_notm}}. Ao visualizar os logs, você vê uma mensagem de log longa. Além disso, no Kibana, você pode ser capaz de ver apenas os últimos 600 a 700 caracteres da mensagem de log.

{: tsCauses}
Uma mensagem de log longa pode ser truncada devido a seu comprimento antes de ser coletada pelo Fluentd, de modo que o log pode não ser analisado corretamente pelo Fluentd antes de ser encaminhado para o {{site.data.keyword.loganalysisshort_notm}}.

{: tsResolve}
Para limitar o comprimento da linha, é possível configurar o seu próprio criador de logs para ter um comprimento máximo para o `stack_trace` em cada log. Por exemplo, se você estiver usando o Log4j para o seu criador de logs, será possível usar um [`EnhancedPatternLayout` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://logging.apache.org/log4j/1.2/apidocs/org/apache/log4j/EnhancedPatternLayout.html) para limitar o `stack_trace` a 15 KB.

## Obtendo ajuda e suporte
{: #health_getting_help}

Ainda está tendo problemas com o seu cluster?
{: shortdesc}

-  No terminal, você é notificado quando atualizações para a CLI `ibmcloud` e plug-ins estão disponíveis. Certifique-se de manter sua CLI atualizada para que seja possível usar todos os comandos e sinalizações disponíveis.
-   Para ver se o {{site.data.keyword.Bluemix_notm}} está disponível, [verifique a página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/status?selected=status).
-   Poste uma pergunta no [{{site.data.keyword.containerlong_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com).
    Se você não estiver usando um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para essa Folga.
    {: tip}
-   Revise os fóruns para ver se outros usuários tiveram o mesmo problema. Ao usar os fóruns para fazer uma pergunta, marque sua pergunta para que ela seja vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.
    -   Se você tiver questões técnicas sobre como desenvolver ou implementar clusters ou apps com o {{site.data.keyword.containerlong_notm}}, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") ](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e identifique-a com `ibm-cloud`, `kubernetes` e `containers`.
    -   Para perguntas sobre o serviço e instruções de introdução, use o fórum do [IBM Developer Answers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Inclua as tags `ibm-cloud` e `containers`.
    Consulte
[Obtendo
ajuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obter mais detalhes sobre o uso dos fóruns.
-   Entre em contato com o Suporte IBM abrindo um caso. Para saber mais sobre como abrir um caso de suporte IBM ou sobre os níveis de suporte e as severidades do caso, consulte [Entrando em contato com o suporte](/docs/get-support?topic=get-support-getting-customer-support).
Ao relatar um problema, inclua o ID do cluster. Para obter o ID do seu cluster, execute `ibmcloud ks clusters`. É possível também usar o [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para reunir e exportar informações pertinentes de seu cluster para compartilhar com o Suporte IBM.
{: tip}

