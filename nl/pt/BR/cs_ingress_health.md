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


# Criação de Log e Monitoramento de Ingresso
{: #ingress_health}

Customize a criação de log e configure o monitoramento para ajudar a solucionar problemas e melhorar o desempenho de sua configuração do Ingress.
{: shortdesc}

## Visualizando logs do Ingresso
{: #ingress_logs}

Se você desejar solucionar problemas do Ingress ou monitorar a atividade do Ingress, será possível revisar os logs do Ingress.
{: shortdesc}

Os logs são coletados automaticamente para seus ALBs do Ingress. Para visualizar os logs do ALB, escolha entre duas opções.
* [Crie uma configuração de criação de log para o serviço Ingress](/docs/containers?topic=containers-health#configuring) em seu cluster.
* Verifique os logs a partir da CLI. **Nota**: deve-se ter pelo menos a [função de serviço **Leitor** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace `kube-system`.
    1. Obtenha o ID de um pod para um ALB.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Abra os logs para esse pod do ALB.
        ```
        kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

</br>O conteúdo de log padrão do Ingress é formatado em JSON e exibe campos comuns que descrevem a sessão de conexão entre um cliente e seu aplicativo. Um log de exemplo com os campos padrão é semelhante ao seguinte:

```
{"time_date": "2018-08-21T17:33:19+00:00", "client": "108.162.248.42", "host": "albhealth.multizone.us-south.containers.appdomain.cloud", "scheme": "http", "request_method": "GET", "request_uri": "/", "request_id": "c2bcce90cf2a3853694da9f52f5b72e6", "status": 200, "upstream_addr": "192.168.1.1:80", "upstream_status": 200, "request_time": 0.005, "upstream_response_time": 0.005, "upstream_connect_time": 0.000, "upstream_header_time": 0.005}
```
{: screen}

<table>
<caption>Entendendo os campos no formato de log do Ingress padrão</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo os campos no formato de log do Ingress padrão</th>
</thead>
<tbody>
<tr>
<td><code> "time_date": "$time_iso8601" </code></td>
<td>O horário local no formato padrão ISO 8601 quando o log é gravado.</td>
</tr>
<tr>
<td><code> "client": "$remote_addr" </code></td>
<td>O endereço IP do pacote de solicitação que o cliente enviou ao seu app. Esse IP pode mudar com base nas situações a seguir:<ul><li>Quando uma solicitação do cliente para o seu app for enviada para o seu cluster, a solicitação será roteada para um pod para o serviço do balanceador de carga que expõe o ALB. Se nenhum pod de app existir no mesmo nó do trabalhador que o pod de serviço de balanceador de carga, o balanceador de carga encaminhará a solicitação para um pod de app em um nó do trabalhador diferente. O endereço IP de origem do pacote de solicitação é mudado para o endereço IP público do nó do trabalhador no qual o pod de app está em execução.</li><li>Se a [preservação de IP de origem estiver ativada](/docs/containers?topic=containers-ingress#preserve_source_ip), o endereço IP original da solicitação do cliente para seu app será registrado no lugar.</li></ul></td>
</tr>
<tr>
<td><code> "host": "$http_host" </code></td>
<td>O host, ou subdomínio, por meio do qual seus apps são acessíveis. Esse subdomínio é configurado nos arquivos de recursos do Ingress para seus ALBs.</td>
</tr>
<tr>
<td><code> "esquema": "$scheme" </code></td>
<td>O tipo de solicitação: <code>HTTP</code> ou <code>HTTPS</code>.</td>
</tr>
<tr>
<td><code> "request_method": "$request_method" </code></td>
<td>O método da chamada de solicitação para o aplicativo back-end, como <code>GET</code> ou <code>POST</code>.</td>
</tr>
<tr>
<td><code> "request_uri": "$uri" </code></td>
<td>O URI da solicitação original para seu caminho de app. Os ALBs processam os caminhos que os apps atendem como prefixos. Quando um ALB recebe uma solicitação de um cliente para um app, o ALB verifica o recurso Ingress para um caminho (como um prefixo) que corresponde ao caminho no URI da solicitação.</td>
</tr>
<tr>
<td><code> "request_id": "$request_id" </code></td>
<td>Um identificador de solicitação exclusivo gerado a partir de 16 bytes aleatórios.</td>
</tr>
<tr>
<td><code>"status": $status</code></td>
<td>O código de status para a sessão de conexão.<ul>
<li><code> 200 </code>: Sessão concluída com sucesso</li>
<li><code>400</code>: os dados de cliente não podem ser analisados</li>
<li><code>403</code>: acesso proibido; por exemplo, quando o acesso é limitado para determinados endereços IP do cliente</li>
<li><code> 500 </code>: erro do servidor interno</li>
<li><code>502</code>: gateway ruim; por exemplo, se um servidor de envio de dados não puder ser selecionado ou atingido</li>
<li><code>503</code>: serviço indisponível; por exemplo, quando o acesso é limitado pelo número de conexões</li>
</ul></td>
</tr>
<tr>
<td><code> "upstream_addr": "$upstream_addr" </code></td>
<td>O endereço IP e a porta ou o caminho para o soquete do domínio do UNIX do servidor de envio de dados. Se vários servidores forem contatados durante o processamento de solicitação, seus endereços serão separados por vírgulas: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock"</code>. Se a solicitação for redirecionada internamente de um grupo de servidores para outro, os endereços do servidor de diferentes grupos serão separados por dois-pontos: <code>"192.168.1.1:80, 192.168.1.2:80, unix:/tmp/sock : 192.168.10.1:80, 192.168.10.2:80"</code>. Se o ALB não puder selecionar um servidor, o nome do grupo de servidores será registrado no lugar.</td>
</tr>
<tr>
<td><code> "upstream_status": $upstream_status </code></td>
<td>O código de status da resposta obtida do servidor de envio de dados para o aplicativo back-end, como os códigos de resposta HTTP padrão. Os códigos de status de várias respostas são separados por vírgulas e dois-pontos como endereços na variável <code>$upstream_addr</code>. Se o ALB não puder selecionar um servidor, o código de status 502 (Gateway ruim) será registrado.</td>
</tr>
<tr>
<td><code> "request_time": $request_time </code></td>
<td>O tempo de processamento da solicitação, medido em segundos com uma resolução de milissegundos. Esse tempo inicia quando o ALB lê os primeiros bytes da solicitação do cliente e para quando o ALB envia os últimos bytes da resposta para o cliente. O log será gravado imediatamente após o tempo de processamento da solicitação parar.</td>
</tr>
<tr>
<td><code> "upstream_response_time": $upstream_response_time </code></td>
<td>O tempo que leva o ALB para receber a resposta do servidor de envio de dados para o aplicativo back-end, medido em segundos com uma resolução de milissegundos. Os tempos de várias respostas são separados por vírgulas e dois-pontos como endereços na variável <code>$upstream_addr</code>.</td>
</tr>
<tr>
<td><code> "upstream_connect_time": $upstream_connect_time </code></td>
<td>O tempo que leva o ALB para estabelecer uma conexão com o servidor de envio de dados para o aplicativo back-end, medido em segundos com uma resolução de milissegundos. Se o TLS/SSL estiver ativado em sua configuração do recurso Ingress, esse tempo incluirá o tempo gasto no handshake. Os tempos de várias conexões são separados por vírgulas e dois-pontos como endereços na variável <code>$upstream_addr</code>.</td>
</tr>
<tr>
<td><code> "upstream_header_time": $upstream_header_time </code></td>
<td>O tempo que leva o ALB para receber o cabeçalho de resposta do servidor de envio de dados para o aplicativo backend, medido em segundos com uma resolução de milissegundos. Os tempos de várias conexões são separados por vírgulas e dois-pontos como endereços na variável <code>$upstream_addr</code>.</td>
</tr>
</tbody></table>

## Customizando o conteúdo e o formato de log do Ingress
{: #ingress_log_format}

É possível customizar o conteúdo e o formato de logs que são coletados para o ALB do Ingress.
{:shortdesc}

Por padrão, os logs do Ingress são formatados em JSON e exibem campos de log comuns. No entanto, também é possível criar um formato de log customizado escolhendo quais componentes de log são encaminhados e como os componentes são organizados na saída de log

Antes de iniciar, assegure-se de que tenha a [função de serviço **Gravador** ou **Gerenciador** do IAM do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#platform) para o namespace `kube-system`.

1. Edite o arquivo de configuração para o recurso configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Inclua um <code>dados</code> seção. Inclua o campo `log-format` e, opcionalmente, o campo `log-format-escape-json`.

    ```
    apiVersion: v1
    data:
      log-format: '{<key1>: <log_variable1>, <key2>: <log_variable2>, <key3>: <log_variable3>}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

    <table>
    <caption>Componentes de arquivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone ideia"/> Entendendo a configuração de formato de log</th>
    </thead>
    <tbody>
    <tr>
    <td><code>log-format</code></td>
    <td>Substitua <code>&lt;key&gt;</code> pelo nome do componente de log e <code>&lt;log_variable&gt;</code> por uma variável para o componente de log que você deseja coletar em entradas de log. É possível incluir texto e pontuação que você deseja que a entrada de log contenha, como aspas em torno de valores de sequência e vírgulas para separar os componentes de log. Por exemplo, formatar um componente como <code>request: "$request"</code> gera o seguinte em uma entrada de log: <code>request: "GET / HTTP/1.1"</code>. Para obter uma lista de todas as variáveis que podem ser usadas, consulte o <a href="http://nginx.org/en/docs/varindex.html">Índice de variável NGINX</a>.<br><br>Para registrar um cabeçalho adicional como <em>x-custom-ID</em>, inclua o par chave-valor a seguir no conteúdo de log customizado: <br><pre class="codeblock"><code>customID: $http_x_custom_id</code></pre> <br>Os hifens (<code>-</code>) são convertidos em sublinhados (<code>_</code>) e <code>$http_</code> deve ser pré-anexado ao nome do cabeçalho customizado.</td>
    </tr>
    <tr>
    <td><code>log-format-escape-json</code></td>
    <td>Opcional: por padrão, os logs são gerados em formato de texto. Para gerar logs no formato JSON, inclua o campo <code>log-format-escape-json</code> e use o valor <code>true</code>.</td>
    </tr>
    </tbody></table>

    Por exemplo, seu formato de log pode conter as variáveis a seguir:
    ```
    apiVersion: v1
    data:
      log-format: '{remote_address: $remote_addr, remote_user: "$remote_user",
                    time_date: [$time_local], request: "$request",
                    status: $status, http_referer: "$http_referer",
                    http_user_agent: "$http_user_agent",
                    request_id: $request_id}'
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: screen}

    Uma entrada de log de acordo com esse formato é semelhante ao exemplo a seguir:
    ```
    remote_address: 127.0.0.1, remote_user: "dbmanager", time_date: [30/Mar/2018:18:52:17 +0000], request: "GET / HTTP/1.1", status: 401, http_referer: "-", http_user_agent: "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:47.0) Gecko/20100101 Firefox/47.0", request_id: a02b2dea9cf06344a25611c1d7ad72db
    ```
    {: screen}

    Para criar um formato de log customizado que é baseado no formato padrão para logs do ALB, modifique a seção a seguir conforme necessário e inclua-a em seu configmap:
    ```
    apiVersion: v1
    data:
      log-format: '{"time_date": "$time_iso8601", "client": "$remote_addr",
                    "host": "$http_host", "scheme": "$scheme",
                    "request_method": "$request_method", "request_uri": "$uri",
                    "request_id": "$request_id", "status": $status,
                    "upstream_addr": "$upstream_addr", "upstream_status":
                    $upstream_status, "request_time": $request_time,
                    "upstream_response_time": $upstream_response_time,
                    "upstream_connect_time": $upstream_connect_time,
                    "upstream_header_time": $upstream_header_time}'
      log-format-escape-json: "true"
    kind: ConfigMap
    metadata:
      name: ibm-cloud-provider-ingress-cm
      namespace: kube-system
    ```
    {: codeblock}

4. Salve o arquivo de configuração.

5. Verifique se as mudanças de configmap foram aplicadas.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}

4. Para visualizar os logs do ALB do Ingress, escolha entre duas opções.
    * [Crie uma configuração de criação de log para o serviço Ingress](/docs/containers?topic=containers-health#logging) em seu cluster.
    * Verifique os logs a partir da CLI.
        1. Obtenha o ID de um pod para um ALB.
            ```
            kubectl get pods -n kube-system | grep alb
            ```
            {: pre}

        2. Abra os logs para esse pod do ALB. Verifique se os logs seguem o formato atualizado.
            ```
            kubectl logs <ALB_pod_ID> nginx-ingress -n kube-system
            ```
            {: pre}

<br />


## Monitorando o ALB de Ingresso
{: #ingress_monitoring}

Monitore seus ALBs implementando um exportador de métricas e o agente Prometheus em seu cluster.
{: shortdesc}

O exportador de métricas ALB usa a diretiva NGINX, `vhost_traf_status_zone`, para coletar dados de métricas do terminal `/status/format/json` em cada pod do ALB do Ingress. O exportador de métricas reformatará automaticamente cada campo de dados no arquivo JSON para uma métrica que seja legível para o Prometheus. Em seguida, um agente Prometheus seleciona as métricas produzidas pelo exportador e torna as métricas visíveis em um painel Prometheus.

### Instalando o gráfico do Helm do exportador de métricas
{: #metrics-exporter}

Instale o gráfico do Helm do exportador de métricas para monitorar um ALB em seu cluster.
{: shortdesc}

Os pods exportadores de métricas do ALB devem implementar nos mesmos nós do trabalhador para os quais seus ALBs são implementados. Se os seus ALBs são executados em nós do trabalhador de borda e esses nós de borda são contaminados para evitar outras implementações de carga de trabalho, os pods exportadores de métricas não poderão ser planejados. Deve-se remover as contaminações executando `kubectl taint node <node_name> dedicated:NoSchedule- dedicated:NoExecute-`.
{: note}

1.  **Importante**: [siga as instruções](/docs/containers?topic=containers-helm#public_helm_install) para instalar o cliente do Helm em sua máquina local e o servidor do Helm (tiller) com uma conta de serviço e incluir os repositórios do Helm do {{site.data.keyword.Bluemix_notm}}.

2. Instale o gráfico do Helm `ibmcloud-alb-metrics-exporter` em seu cluster. Esse gráfico do Helm implementa um exportador de métricas do ALB e cria uma conta de serviço `alb-metrics-service-account` no namespace `kube-system`. Substitua <alb-ID > pelo ID do ALB para o qual você deseja coletar as métricas. Para visualizar os IDs para os ALBs em seu cluster, execute <code>ibmcloud ks albs --cluster &lt;cluster_name&gt;</code>.
  Deve-se implementar um gráfico para cada ALB que será monitorado.
  {: note}
  ```
  helm install iks-charts/ibmcloud-alb-metrics-exporter --name ibmcloud-alb-metrics-exporter --set metricsNameSpace=kube-system --set albId=<alb-ID>
  ```
  {: pre}

3. Verifique o status de implementação do gráfico. Quando o gráfico estiver pronto, o campo **STATUS** perto da parte superior da saída terá um valor de `DEPLOYED`.
  ```
  helm status ibmcloud-alb-metrics-exporter
  ```
  {: pre}

4. Verifique se os pods `ibmcloud-alb-metrics-exporter` estão em execução.
  ```
  kubectl get pods -n kube-system -o wide
  ```
  {:pre}

  Saída de exemplo:
  ```
  NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
  ...
  alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
  ```
  {:screen}

5. Opcional: [instale o agente do Prometheus](#prometheus-agent) para selecionar as métricas produzidas pelo exportador e torná-las visíveis em um painel do Prometheus.

### Instalando o gráfico do Helm do agente do Prometheus
{: #prometheus-agent}

Depois de instalar o [exportador de métricas](#metrics-exporter), será possível instalar o gráfico do Helm do agente do Prometheus para selecionar as métricas produzidas pelo exportador e torná-las visíveis em um painel do Prometheus.
{: shortdesc}

1. Faça download do arquivo TAR para o gráfico do Helm do exportador de métricas em https://icr.io/helm/iks-charts/charts/ibmcloud-alb-metrics-exporter-1.0.7.tgz

2. Navegue até a subpasta do Prometheus.
  ```
  cd ibmcloud-alb-metrics-exporter-1.0.7.tar/ibmcloud-alb-metrics-exporter/subcharts/prometheus
  ```
  {: pre}

3. Instale o gráfico do Helm do Prometheus em seu cluster. Substitua <ingress_subdomain> pelo subdomínio Ingress para seu cluster. A URL para o painel do Prometheus é uma combinação do subdomínio padrão do Prometheus, `prom-dash`, e do seu subdomínio do Ingress, por exemplo, `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. Para localizar o subdomínio do Ingress para seu cluster, execute <code>ibmcloud ks cluster-get --cluster &lt;cluster_name&gt;</code>.
  ```
  helm install --name prometheus . --set nameSpace=kube-system --set hostName=prom-dash.<ingress_subdomain>
  ```
  {: pre}

4. Verifique o status de implementação do gráfico. Quando o gráfico estiver pronto, o campo **STATUS** perto da parte superior da saída terá um valor de `DEPLOYED`.
    ```
    helm status prometheus
    ```
    {: pre}

5. Verifique se o pod `prometheus` está em execução.
    ```
    kubectl get pods -n kube-system -o wide
    ```
    {:pre}

    Saída de exemplo:
    ```
    NAME                                             READY     STATUS      RESTARTS   AGE       IP               NODE
    alb-metrics-exporter-868fddf777-d49l5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    alb-metrics-exporter-868fddf777-pf7x5            1/1       Running     0          19s       172.30.xxx.xxx   10.xxx.xx.xxx
    prometheus-9fbcc8bc7-2wvbk                       1/1       Running     0          1m        172.30.xxx.xxx   10.xxx.xx.xxx
    ```
    {:screen}

6. Em um navegador, insira a URL para o painel Prometheus. Esse nome do host tem o formato `prom-dash.mycluster-12345.us-south.containers.appdomain.cloud`. O painel Prometheus para seu ALB é aberto.

7. Revise mais informações sobre as métricas do [ALB](#alb_metrics), do [servidor](#server_metrics) e do [envio de dados](#upstream_metrics) listadas no painel.

### Métricas do ALB
{: #alb_metrics}

O `alb-metrics-exporter` reformata automaticamente cada campo de dados no arquivo JSON em uma métrica que é legível pelo Prometheus. As métricas do ALB coletam dados sobre as conexões e respostas que o ALB está manipulando.
{: shortdesc}

As métricas do ALB estão no formato `kube_system_<ALB-ID>_<METRIC-NAME> <VALUE>`. Por exemplo, se um ALB receber 23 respostas com códigos de status de nível 2xx, a métrica será formatada como `kube_system_public_crf02710f54fcc40889c301bfd6d5b77fe_alb1_totalHandledRequest {.. metric="2xx"} 23` em que `metric` é o rótulo do Prometheus.

A tabela a seguir lista os nomes das métricas do ALB suportadas com os rótulos de métrica no formato `<ALB_metric_name>_<metric_label>`
<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/>  Métricas do ALB Suportadas</th>
</thead>
<tbody>
<tr>
<td><code> connections_reading </code></td>
<td>O número total de conexões do cliente de leitura.</td>
</tr>
<tr>
<td><code> connections_aceitou </code></td>
<td>O número total de conexões do cliente aceitas.</td>
</tr>
<tr>
<td><code> connections_active </code></td>
<td>O número total de conexões do cliente ativas.</td>
</tr>
<tr>
<td><code> connections_manipulado </code></td>
<td>O número total de conexões do cliente manipuladas.</td>
</tr>
<tr>
<td><code> connections_requests </code></td>
<td>O número total de conexões do cliente solicitadas.</td>
</tr>
<tr>
<td><code> connections_UNK </code></td>
<td>O número total de conexões do cliente em espera.</td>
</tr>
<tr>
<td><code>connections_gravando</code></td>
<td>O número total de conexões do cliente de gravação.</td>
</tr>
<tr>
<td><code> totalHandledRequest_1xx </code></td>
<td>O número de respostas com códigos de status 1xx.</td>
</tr>
<tr>
<td><code> totalHandledRequest_2xx </code></td>
<td>O número de respostas com códigos de status 2xx.</td>
</tr>
<tr>
<td><code> totalHandledRequest_3xx </code></td>
<td>O número de respostas com códigos de status 3xx.</td>
</tr>
<tr>
<td><code> totalHandledRequest_4xx </code></td>
<td>O número de respostas com códigos de status 4xx.</td>
</tr>
<tr>
<td><code> totalHandledRequest_5xx </code></td>
<td>O número de respostas com códigos de status 5xx.</td>
</tr>
<tr>
<td><code> totalHandledRequest_total </code></td>
<td>O número total de solicitações do cliente recebidas de clientes.</td>
  </tr></tbody>
</table>

### Métricas do servidor
{: #server_metrics}

O `alb-metrics-exporter` reformata automaticamente cada campo de dados no arquivo JSON em uma métrica que é legível pelo Prometheus. As métricas do servidor coletam dados no subdomínio definido em um recurso do Ingress; por exemplo, `dev.demostg1.stg.us.south.containers.appdomain.cloud`.
{: shortdesc}

As métricas do servidor estão no formato `kube_system_server_<ALB-ID>_<SUB-TYPE>_<SERVER-NAME>_<METRIC-NAME> <VALUE>`.

`<SERVER-NAME>_<METRIC-NAME>` são formatadas como rótulos. Por exemplo,  ` albId="dev_demostg1_us-south_containers_appdomain_cloud ", metric="out" `

Por exemplo, se o servidor enviou um total de 22319 bytes para clientes, a métrica será formatada como:
```
kube_system_public_public_cra6a6eb9e897e41c4a5e58f957b417aec_bytes {1}_us-south_containers_appdomain_cloud ", instance="172.30.140.68:9913", job="kubernetes_namespace="kubernetes_pod_name="alb-metrics-7d495d785c-8wfw4 ", metric="out", pod_template_hash="3805183417 " } 22319
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo o formato de métrica do servidor</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>O ID do ALB. No exemplo acima, o ID do ALB é <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<tr>
<td><code> &lt;SUB-TYPE&gt; </code></td>
<td>O subtipo da métrica. Cada subtipo corresponde a um ou mais nomes de métrica.
<ul>
<li><code>bytes</code> e <code>processing\_time</code> correspondem às métricas <code>in</code> e <code>out</code>.</li>
<li><code>cache</code> corresponde às métricas <code>bypass</code>, <code>expired</code>, <code>hit</code>, <code>miss</code>, <code>revalidated</code>, <code>scare</code>, <code>stale</code> e <code>updating</code>.</li>
<li><code>requests</code> corresponde às métricas <code>requestMsec</code>, <code>1xx</code>, <code>2xx</code>, <code>3xx</code>, <code>4xx</code>, <code>5xx</code> e <code>total</code>.</li></ul>
No exemplo acima, o subtipo é <code>bytes</code>.</td>
</tr>
<tr>
<td><code> &lt;SERVER-NAME&gt; </code></td>
<td>O nome do servidor que está definido no recurso Ingress. Para manter a compatibilidade com o Prometheus, os pontos (<code>.</code>) são substituídos por sublinhados <code>(\_)</code>. No exemplo acima, o nome do servidor é <code>dev_demostg1_stg_us_south_containers_appdomain_cloud</code>.</td>
</tr>
<tr>
<td><code> &lt;METRIC_NAME&gt; </code></td>
<td>O nome do tipo de métrica coletado. Para obter uma lista de nomes de métricas, consulte a tabela a seguir "Métricas do servidor suportadas". No exemplo acima, o nome da métrica é <code>out</code>.</td>
</tr>
<tr>
<td><code> &lt;VALUE&gt; </code></td>
<td>O valor da métrica coletada. No exemplo acima, o valor é <code>22319</code>.</td>
</tr>
</tbody></table>

A tabela a seguir lista os nomes de métrica do servidor suportados.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/>  Métricas do Servidor Suportadas</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>O número total de bytes recebidos de clientes.</td>
</tr>
<tr>
<td><code> out </code></td>
<td>O número total de bytes enviados para os clientes.</td>
</tr>
<tr>
<td><code> bypass </code></td>
<td>O número de vezes que um item armazenável em cache foi buscado por meio do servidor de origem porque ele não atendeu ao limite para estar no cache (por exemplo, número de solicitações).</td>
</tr>
<tr>
<td><code> expirado </code></td>
<td>O número de vezes que um item foi localizado no cache, mas não foi selecionado porque estava expirado.</td>
</tr>
<tr>
<td><code> acertos </code></td>
<td>O número de vezes que um item válido foi selecionado por meio do cache.</td>
</tr>
<tr>
<td><code> miss </code></td>
<td>O número de vezes que nenhum item de cache válido foi localizado no cache e o servidor buscou o item por meio do servidor de origem.</td>
</tr>
<tr>
<td><code> revalidado </code></td>
<td>O número de vezes que um item expirado no cache foi revalidado.</td>
</tr>
<tr>
<td><code> scarce </code></td>
<td>O número de vezes que o cache removeu os itens raramente usados ou de baixa prioridade para liberar a memória escassa.</td>
</tr>
<tr>
<td><code> stale </code></td>
<td>O número de vezes que um item expirado foi localizado no cache, mas como outra solicitação fez com que o servidor buscasse o item por meio do servidor de origem, o item foi selecionado do cache.</td>
</tr>
<tr>
<td><code>atualizando</code></td>
<td>O número de vezes que o conteúdo antigo foi atualizado.</td>
</tr>
<tr>
<td><code> requestMsec </code></td>
<td>A média de tempos de processamento de solicitação em milissegundos.</td>
</tr>
<tr>
<td><code> 1xx </code></td>
<td>O número de respostas com códigos de status 1xx.</td>
</tr>
<tr>
<td><code> 2xx </code></td>
<td>O número de respostas com códigos de status 2xx.</td>
</tr>
<tr>
<td><code> 3xx </code></td>
<td>O número de respostas com códigos de status 3xx.</td>
</tr>
<tr>
<td><code> 4xx </code></td>
<td>O número de respostas com códigos de status 4xx.</td>
</tr>
<tr>
<td><code> 5xx </code></td>
<td>O número de respostas com códigos de status 5xx.</td>
</tr>
<tr>
<td><code> total </code></td>
<td>O número total de respostas com códigos de status.</td>
  </tr></tbody>
</table>

### Métricas de envio de dados
{: #upstream_metrics}

O `alb-metrics-exporter` reformata automaticamente cada campo de dados no arquivo JSON em uma métrica que é legível pelo Prometheus. As métricas de envio de dados coletam dados sobre o serviço de back-end definido em um recurso do Ingress.
{: shortdesc}

As métricas de envio de dados são formatadas de duas maneiras.
* [Tipo 1](#type_one) inclui o nome do serviço de envio de dados.
* [Tipo 2](#type_two) inclui o nome do serviço de envio de dados e um endereço IP do pod de envio de dados específico.

#### Métricas de envio de dados do tipo 1
{: #type_one}

As métricas de envio de dados de tipo 1 estão no formato `kube_system_upstream_<ALB-ID>_<SUB-TYPE>_<UPSTREAM-NAME>_<METRIC-NAME> <VALUE>`.
{: shortdesc}

`<UPSTREAM-NAME>_<METRIC-NAME>` são formatadas como rótulos. Por exemplo,  ` albId="default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc ", metric="in" `

Por exemplo, se o serviço de envio de dados recebeu um total de 1227 bytes do ALB, a métrica será formatada como:
```
kube_system_public_cra6a6eb9e897e41c4a5e58f957b417aec_bytes {1}{7}{2}.30.140.68:9913", job="kubernetes_namespace="kube-system "exportado-7d495d785c-8wfw4", metric="in ", pod_template_hash="3805183417" } 1227
```
{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/>Entendendo o formato de métrica do tipo 1 de envio de dados</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>O ID do ALB. No exemplo acima, o ID do ALB é <code>public\_crf02710f54fcc40889c301bfd6d5b77fe\_alb1</code>.</td>
</tr>
<tr>
<td><code> &lt;SUB-TYPE&gt; </code></td>
<td>O subtipo da métrica. Os valores suportados são <code>bytes</code>, <code>processing\_time</code> e <code>requests</code>. No exemplo acima, o subtipo é <code>bytes</code>.</td>
</tr>
<tr>
<td><code> &lt;UPSTREAM-NAME&gt; </code></td>
<td>O nome do serviço de envio de dados que é definido no recurso Ingress. Para manter a compatibilidade com o Prometheus, os pontos (<code>.</code>) são substituídos por sublinhados <code>(\_)</code>. No exemplo acima, o nome do envio de dados é <code>default-cafe-ingress-dev_demostg1_us-south_containers_appdomain_cloud-coffee-svc</code>.</td>
</tr>
<tr>
<td><code> &lt;METRIC_NAME&gt; </code></td>
<td>O nome do tipo de métrica coletado. Para obter uma lista de nomes de métricas, consulte a tabela a seguir "Métricas de tipo 1 de envio de dados suportadas". No exemplo acima, o nome da métrica é <code>in</code>.</td>
</tr>
<tr>
<td><code> &lt;VALUE&gt; </code></td>
<td>O valor da métrica coletada. No exemplo acima, o valor é <code>1227</code>.</td>
</tr>
</tbody></table>

A tabela a seguir lista os nomes de métrica do tipo 1 de envio de dados suportados.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Métricas de tipo 1 de envio de dados suportadas</th>
</thead>
<tbody>
<tr>
<td><code>in</code></td>
<td>O número total de bytes recebidos do servidor ALB.</td>
</tr>
<tr>
<td><code> out </code></td>
<td>O número total de bytes enviados para o servidor ALB.</td>
</tr>
<tr>
<td><code> 1xx </code></td>
<td>O número de respostas com códigos de status 1xx.</td>
</tr>
<tr>
<td><code> 2xx </code></td>
<td>O número de respostas com códigos de status 2xx.</td>
</tr>
<tr>
<td><code> 3xx </code></td>
<td>O número de respostas com códigos de status 3xx.</td>
</tr>
<tr>
<td><code> 4xx </code></td>
<td>O número de respostas com códigos de status 4xx.</td>
</tr>
<tr>
<td><code> 5xx </code></td>
<td>O número de respostas com códigos de status 5xx.</td>
</tr>
<tr>
<td><code> total </code></td>
<td>O número total de respostas com códigos de status.</td>
  </tr></tbody>
</table>

#### Métricas de envio de dados do tipo 2
{: #type_two}

As métricas de envio de dados de tipo 2 estão no formato `kube_system_upstream_<ALB-ID>_<METRIC-NAME>_<UPSTREAM-NAME>_<POD-IP> <VALUE>`.
{: shortdesc}

`<UPSTREAM-NAME>_<POD-IP>` são formatadas como rótulos. Por exemplo,  ` albId="default-cafe-ingress-dev_dev_demostg1_us-south_containers_appdomain_cloud-tea-svc ", backend="172_30_75_6_80" `

Por exemplo, se o serviço de envio de dados tiver um tempo médio de processamento de solicitação (incluindo o envio de dados) de 40 milissegundos, a métrica será formatada como:
```
kube_system_public_cra6a6eb9e897e41c4a5e58f957b417aec_alb1_requestMsec {1}{7}{2}.30.75.3:9913", job="kubernetespace="kube-system ", kubernetes_pod_name="alb-metrics-7d495d785c-swkls", pod_template_hash="3805183417 " } 40
```

{: screen}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Entendendo o formato de métrica do tipo 2 de envio de dados</th>
</thead>
<tbody>
<tr>
<td><code>&lt;ALB-ID&gt;</code></td>
<td>O ID do ALB. No exemplo acima, o ID do ALB é <code>public\_cra6a6eb9e897e41c4a5e58f957b417aec\_alb1</code>.</td>
</tr>
<td><code> &lt;UPSTREAM-NAME&gt; </code></td>
<td>O nome do serviço de envio de dados que é definido no recurso Ingress. Para manter a compatibilidade com o Prometheus, os pontos (<code>.</code>) são substituídos por sublinhados (<code>\_</code>). No exemplo acima, o nome do envio de dados é <code>demostg1\_stg\_us\_south\_containers\_appdomain\_cloud\_tea\_svc</code>.</td>
</tr>
<tr>
<td><code> &lt; POD\_IP &gt; </code></td>
<td>O endereço IP e a porta de um pod de serviço de envio de dados específico. Para manter a compatibilidade com o Prometheus, os pontos (<code>.</code>) e os dois-pontos (<code>:</code>) são substituídos por sublinhados <code>(_)</code>. No exemplo acima, o IP do pod de envio de dados é <code>172_30_75_6_80</code>.</td>
</tr>
<tr>
<td><code> &lt;METRIC_NAME&gt; </code></td>
<td>O nome do tipo de métrica coletado. Para obter uma lista de nomes de métricas, consulte a tabela a seguir "Métricas do tipo 2 de envio de dados suportadas". No exemplo acima, o nome da métrica é <code>requestMsec</code>.</td>
</tr>
<tr>
<td><code> &lt;VALUE&gt; </code></td>
<td>O valor da métrica coletada. No exemplo acima, o valor é <code>40</code>.</td>
</tr>
</tbody></table>

A tabela a seguir lista os nomes de métricas do tipo 2 de envio de dados suportadas.

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Métricas do tipo 2 de envio de dados suportadas</th>
</thead>
<tbody>
<tr>
<td><code> requestMsec </code></td>
<td>A média de tempos de processamento da solicitação, incluindo o envio de dados, em milissegundos.</td>
</tr>
<tr>
<td><code> responseMsec </code></td>
<td>A média de apenas tempos de processamento de resposta de envio de dados em milissegundos.</td>
  </tr></tbody>
</table>

<br />


## Aumentando o tamanho da zona de memória compartilhada para a coleção de métricas do Ingress
{: #vts_zone_size}

As zonas de memória compartilhada são definidas para que os processos do trabalhador possam compartilhar informações, como cache, persistência de sessão e limites de taxa. Uma zona de memória compartilhada, chamada de zona de status de tráfego do host virtual, está configurada para o Ingresso para coletar dados de métricas para um ALB.
{:shortdesc}

No configmap do Ingresso `ibm-cloud-provider-ingress-cm`, o campo `vts-status-zone-size` configura o tamanho da zona de memória compartilhada para a coleta de dados de métrica. Por padrão, `vts-status-zone-size` é configurado como `10m`. Se você tiver um ambiente grande que requeira mais memória para coleta de métricas, será possível substituir o padrão para usar, como alternativa, um valor maior, seguindo estas etapas.

Antes de iniciar, assegure-se de que tenha a [função de serviço **Gravador** ou **Gerenciador** do IAM do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#platform) para o namespace `kube-system`.

1. Edite o arquivo de configuração para o recurso configmap `ibm-cloud-provider-ingress-cm`.

    ```
    kubectl edit cm ibm-cloud-provider-ingress-cm -n kube-system
    ```
    {: pre}

2. Mude o valor de `vts-status-zone-size` de `10m` para um valor maior.

   ```
   apiVersion: v1
   data:
     vts-status-zone-size: "10m"
   kind: ConfigMap
   metadata:
     name: ibm-cloud-provider-ingress-cm
     namespace: kube-system
   ```
   {: codeblock}

3. Salve o arquivo de configuração.

4. Verifique se as mudanças de configmap foram aplicadas.

   ```
   kubectl get cm ibm-cloud-provider-ingress-cm -n kube-system -o yaml
   ```
   {: pre}
