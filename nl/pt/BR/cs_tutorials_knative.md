---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-11"

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


# Tutorial: usando o Knative gerenciado para executar apps sem servidor em clusters do Kubernetes
{: #knative_tutorial}

Com esse tutorial, é possível aprender como instalar o Knative em um cluster Kubernetes no {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**O que é o Knative e por que devo utilizá-lo?**</br>
[Knative](https://github.com/knative/docs) é uma plataforma de software livre que foi desenvolvida pela IBM, Google, Pivotal, Red Hat, Cisco e outras com a meta de estender os recursos do Kubernetes para ajudá-lo a criar apps serveless modernos e conteinerizados centrados na origem na parte superior do cluster Kubernetes. A plataforma é projetada para atender às necessidades de desenvolvedores que hoje devem decidir qual tipo de aplicativo eles desejam executar na nuvem: apps de 12 fatores, contêineres ou funções. Cada tipo de app requer uma solução proprietária ou de software livre que seja customizada para esses apps: Cloud Foundry para apps de 12 fatores, Kubernetes para contêineres e OpenWhisk, e outros para funções. No passado, os desenvolvedores tinham que decidir qual abordagem desejavam seguir, o que levava à inflexibilidade e à complexidade quando diferentes tipos de apps tinham que ser combinados.  

O Knative usa uma abordagem consistente entre linguagens de programação e estruturas para abstrair a carga operacional de construir, implementar e gerenciar cargas de trabalho no Kubernetes para que os desenvolvedores possam se concentrar no que mais importa para eles: o código-fonte. É possível usar pacotes de construção comprovados com os que você já está familiarizado, como o Cloud Foundry, Kaniko, Dockerfile, Bazel e outros. Ao se integrar com o Istio, o Knative assegura que as cargas de trabalho sem servidor e conteinerizadas possam ser facilmente expostas na Internet, monitoradas e controladas e que seus dados sejam criptografados durante o trânsito.

**Como o Knative funciona?**</br>
O Knative é fornecido com três componentes chave ou _primitivas_, que ajudam a construir, implementar e gerenciar seus apps sem servidor no cluster Kubernetes:

- **Construção:** a primitiva `Build` suporta a criação de um conjunto de etapas para construir seu app por meio do código-fonte para uma imagem de contêiner. Imagine usar um modelo de construção simples no qual você especifica o repositório de origem para localizar seu código de app e o registro do contêiner no qual você deseja hospedar a imagem. Com apenas um único comando, é possível instruir o Knative a tomar esse modelo de construção, extrair o código-fonte, criar a imagem e enviá-la por push para seu registro de contêiner para que seja possível usar a imagem em seu contêiner.
- **Servindo:** A primitiva `Serving` ajuda a implementar aplicativos sem servidor como serviços Knativos e para escaloná-los automaticamente, até mesmo para zero instâncias. Usando o gerenciamento de tráfego e os recursos de roteamento inteligentes do Istio, é possível controlar qual tráfego é roteado para uma versão específica de seu serviço, o que facilita para um desenvolvedor testar e lançar uma nova versão de app ou executar um teste de A-B.
- **Evento:** com a primitiva `Eventing`, é possível criar acionadores ou fluxos de eventos que outros serviços podem assinar. Por exemplo, você pode desejar iniciar uma nova construção de seu app sempre que o código for enviado por push para o repositório principal do GitHub. Ou você deseja executar um app sem servidor apenas se a temperatura cair abaixo do ponto de congelamento. A primitiva `Eventing` pode ser integrada em seu pipeline CI/CD para automatizar a construção e a implementação de apps no caso de um evento específico ocorrer.

**O que é o complemento Knative gerenciado no {{site.data.keyword.containerlong_notm}} (experimental)?** </br>
O Knative gerenciado no {{site.data.keyword.containerlong_notm}} é um complemento gerenciado que integra o Knative e o Istio diretamente a seu cluster Kubernetes. As versões Knative e Istio no complemento são testadas pela IBM e suportadas para uso no {{site.data.keyword.containerlong_notm}}. Para obter mais informações sobre os complementos gerenciados, consulte [Incluindo serviços por meio do uso de complementos gerenciados](/docs/containers?topic=containers-managed-addons#managed-addons).

**Há alguma limitação?** </br>
Se você instalou o [controlador de admissão de imposição de segurança da imagem de contêiner](/docs/services/Registry?topic=registry-security_enforce#security_enforce) em seu cluster, não será possível ativar o complemento Knative gerenciado no cluster.

Parece bom? Siga este tutorial para iniciar o Knative no {{site.data.keyword.containerlong_notm}}.

## Objetivos
{: #knative_objectives}

- Aprenda os fundamentos sobre o Knative e as primitivas Knative.  
- Instale o complemento Knative gerenciado e o Istio gerenciado em seu cluster.
- Implemente seu primeiro aplicativo sem servidor com Knative e exponha o aplicativo na Internet usando a primitiva Knative `Serving`.
- Explore os recursos de ajuste de escala e de revisão do Knative.

## Tempo Necessário
{: #knative_time}

30 minutos

## Público
{: #knative_audience}

Este tutorial é projetado para desenvolvedores que estão interessados em aprender como usar o Knative para implementar um app sem servidor em um cluster Kubernetes e para administradores de cluster que desejam aprender como configurar o Knative em um cluster.

## Pré-requisitos
{: #knative_prerequisites}

-  [Instale a CLI do IBM Cloud, o plug-in do {{site.data.keyword.containerlong_notm}} e a CLI do Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Certifique-se de instalar a versão da CLI `kubectl` que corresponde à versão do Kubernetes do seu cluster.
-  [Crie um cluster com pelo menos 3 nós do trabalhador que contenham, cada um, 4 núcleos e 16 GB de memória (`b3c.4x16`) ou mais](/docs/containers?topic=containers-clusters#clusters_cli). Todos os nós do trabalhador devem executar o Kubernetes versão 1.12 ou mais recente.
-  Assegure-se de que você tenha a [função de **Gravador** ou **Gerenciador** do serviço {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.containerlong_notm}}.
-  [Destino a CLI para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

## Lição 1: Configurando o Complemento Knative Gerenciado
{: #knative_setup}

O Knative é construído na parte superior do Istio para assegurar que as cargas de trabalho sem servidor e conteinerizadas possam ser expostas dentro do cluster e na Internet. Com o Istio, é possível também monitorar e controlar o tráfego de rede entre seus serviços e assegurar que seus dados sejam criptografados durante o trânsito. Ao instalar o complemento Knative gerenciado, o complemento Istio gerenciado é instalado automaticamente também.
{: shortdesc}

1. Ative o complemento Knative gerenciado em seu cluster. Ao ativar o Knative em seu cluster, o Istio e todos os componentes Knative são instalados em seu cluster.
   ```
   ibmcloud ks cluster-addon-enable knative --cluster <cluster_name_or_ID> -y
   ```
   {: pre}

   Saída de exemplo:
   ```
   Enabling add-on knative for cluster knative...
   OK
   ```
   {: screen}

   A instalação de todos os componentes Knative pode levar alguns minutos para ser concluída.

2. Verifique se o Istio foi instalado com êxito. Todos os pods para os nove serviços do Istio e o pod para Prometheus devem estar em um status `Em execução`.
   ```
   kubectl get pods -- namespace istio-system
   ```
   {: pre}

   Saída de exemplo:
   ```
   NAME                                       READY     STATUS      RESTARTS   AGE
    istio-citadel-748d656b-pj9bw               1/1       Running     0          2m
    istio-egressgateway-6c65d7c98d-l54kg       1/1       Running     0          2m
    istio-galley-65cfbc6fd7-bpnqx              1/1       Running     0          2m
    istio-ingressgateway-f8dd85989-6w6nj       1/1       Running     0          2m
    istio-pilot-5fd885964b-l4df6               2/2       Running     0          2m
    istio-policy-56f4f4cbbd-2z2bk              2/2       Running     0          2m
    istio-sidecar-injector-646655c8cd-rwvsx    1/1       Running     0          2m
    istio-statsd-prom-bridge-7fdbbf769-8k42l   1/1       Running     0          2m
    istio-telemetry-8687d9d745-mwjbf           2/2       Running     0          2m
    prometheus-55c7c698d6-f4drj                1/1       Running     0          2m
   ```
   {: screen}

3. Opcional: se desejar usar o Istio para todos os aplicativos no namespace `default`, inclua o rótulo `istio-injection=enabled` no namespace. Cada pod de aplicativo sem servidor deve executar um sidecar de proxy do Envoy para que o aplicativo possa ser incluído na malha de serviços do Istio. Esse rótulo permite que o Istio modifique automaticamente a especificação do modelo de pod em novas implementações de aplicativo para que os pods sejam criados com contêineres do sidecar de proxy do Envoy.
  ```
  kubectl label namespace default istio-injection=enabled
  ```
  {: pre}

4. Verifique se todos os componentes Knative foram instalados com êxito.
   1. Verifique se todos os pods do componente Knative `Serving` estão em um estado `Em execução`.  
      ```
      kubectl get pods -- namespace knative-serving
      ```
      {: pre}

      Saída de exemplo:
      ```
      NAME                          READY     STATUS    RESTARTS   AGE
      activator-598b4b7787-ps7mj    2/2       Running   0          21m
      autoscaler-5cf5cfb4dc-mcc2l   2/2       Running   0          21m
      controller-7fc84c6584-qlzk2   1/1       Running   0          21m
      webhook-7797ffb6bf-wg46v      1/1       Running   0          21m
      ```
      {: screen}

   2. Verifique se todos os pods do componente Knative `Build` estão em um estado `Em Execução`.  
      ```
      kubectl get pods -- namespace knative-build
      ```
      {: pre}

      Saída de exemplo:
      ```
      NOME DO NOME DA PRINCIPAIS RESTARTES IDADE
      build-controller-79cb969d89-kdn2b 1/1 Executando 0 21m
      build-webhook-58d685fc58-twwc4 1/1 Executando 0 21m
      ```
      {: screen}

   3. Verifique se todos os pods do componente Knative `Eventing` estão em um estado `Em execução`.
      ```
      kubectl get pods -- namespace knative-eventing
      ```
      {: pre}

      Saída de exemplo:

      ```
      NAME                                            READY     STATUS    RESTARTS   AGE
      eventing-controller-847d8cf969-kxjtm            1/1       Running   0          22m
      in-memory-channel-controller-59dd7cfb5b-846mn   1/1       Running   0          22m
      in-memory-channel-dispatcher-67f7497fc-dgbrb    2/2       Running   1          22m
      webhook-7cfff8d86d-vjnqq                        1/1       Running   0          22m
      ```
      {: screen}

   4. Verifique se todos os pods do componente Knative `Sources` estão em um estado `Em execução`.
      ```
      kubectl get pods -- namespace knative-sources
      ```
      {: pre}

      Saída de exemplo:
      ```
      NAME                   READY     STATUS    RESTARTS   AGE
      controller-manager-0   1/1       Running   0          22m
      ```
      {: screen}

   5. Verifique se todos os pods do componente Knative `Monitoring` estão em um estado `Em execução`.
      ```
      kubectl get pods -- namespace knative-monitoring
      ```
      {: pre}

      Saída de exemplo:
      ```
      NAME                                  READY     STATUS                 RESTARTS   AGE
      elasticsearch-logging-0               1/1       Running                0          22m
      elasticsearch-logging-1               1/1       Running                0          21m
      grafana-79cf95cc7-mw42v               1/1       Running                0          21m
      kibana-logging-7f7b9698bc-m7v6r       1/1       Running                0          22m
      kube-state-metrics-768dfff9c5-fmkkr   4/4       Running                0          21m
      node-exporter-dzlp9                   2/2       Running                0          22m
      node-exporter-hp6gv                   2/2       Running                0          22m
      node-exporter-hr6vs                   2/2       Running                0          22m
      prometheus-system-0                   1/1       Running                0          21m
      prometheus-system-1                   1/1       Running                0          21m
      ```
      {: screen}

Ótimo! Com o Knative e o Istio todos configurados, agora é possível implementar seu primeiro app sem servidor em seu cluster.

## Lição 2: Implementar um app sem servidor em seu cluster
{: #deploy_app}

Nesta lição, você implementa o seu primeiro app [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) sem servidor no Go. Ao enviar uma solicitação para seu app de amostra, ele lê a variável de ambiente `TARGET` e imprime `"Hello ${TARGET}!"`. Se esta variável de ambiente estiver vazia, `"Hello World!"` será retornado.
{: shortdesc}

1. Crie um arquivo YAML para seu primeiro app sem servidor `Hello World` no Knative. Para implementar um app com Knative, deve-se especificar um recurso do serviço Knative. Um serviço é gerenciado pela primitiva `Serving` do Knative e é responsável por gerenciar o ciclo de vida inteiro da carga de trabalho. O serviço assegura que cada implementação tenha uma revisão do Knative, uma rota e uma configuração. Ao atualizar o serviço, uma nova versão do aplicativo é criada e incluída no histórico de revisão do serviço. As rotas nativas asseguram que cada revisão do app seja mapeada para um terminal de rede para que seja possível controlar a quantidade de tráfego de rede que é roteada para uma revisão específica. As configurações do Knative contêm as configurações de uma revisão específica para que seja possível sempre retroceder para uma revisão mais antiga ou alternar entre revisões. Para obter mais informações sobre os recursos `Serving` do Knative, consulte a [Documentação do Knative](https://github.com/knative/docs/tree/master/serving).
   ```
   apiVersion: serving.knative.dev/v1alpha1
   tipo: Serviço
   metadados:
     nome: kn-helloworld
     namespace: padrão
   spec:
     runLatest:
       configuração:
         revisionTemplate:
           spec:
             contentor:
               imagem: docker.io/ibmcom/kn-helloworld
               env:
               -nome: TARGET
                 valor: "Go Sample v1"
    ```
    {: codeblock}

    <table>
    <caption>Entendendo os componentes de arquivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code>metadata.name</code></td>
    <td>O nome de seu serviço Knative.</td>
    </tr>
    <tr>
    <td><code>metadata.namespace</td>
    <td>O namespace do Kubernetes no qual você deseja implementar o app como um serviço Knative. </td>
    </tr>
    <tr>
    <td><code> spec.container.image </code></td>
    <td>A URL para o registro do contêiner no qual sua imagem está armazenada. Neste exemplo, você implementa um app Hello World do Knative que está armazenado no namespace <code>ibmcom</code> no Docker Hub. </td>
    </tr>
    <tr>
    <td><code> spec.container.env </code></td>
    <td>Uma lista de variáveis de ambiente que você deseja que o seu serviço Knative tenha. Neste exemplo, o valor da variável de ambiente <code>TARGET</code> é lido pelo aplicativo de amostra e retornado quando você envia uma solicitação para seu app no formato <code>"Hello ${TARGET}!"</code>. Se nenhum valor for fornecido, o app de amostra retornará <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Crie o serviço Knative em seu cluster. Ao criar o serviço, a primitiva `Serving` do Knative cria uma revisão imutável, uma rota do Knative, uma regra de roteamento do Ingress, um serviço do Kubernetes, um pod do Kubernetes e um balanceador de carga para seu app. Um subdomínio do seu subdomínio do Ingress é designado ao seu aplicativo no formato `<knative_service_name>.<namespace>.<ingress_subdomain>`, que pode ser usado para acessar o aplicativo por meio da Internet.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Saída de exemplo:
   ```
   service.serving.knative.dev "kn-helloworld" criado
   ```
   {: screen}

3. Verifique se o pod foi criado. Sua pod consiste em dois contêineres. Um contêiner executa seu app `Hello World` e o outro contêiner é um sidecar que executa as ferramentas de monitoramento e criação de log do Istio e do Knative. Seu pod recebe o número de revisão `00001`.
   ```
   kubectl get pods
   ```
   {: pre}

   Saída de exemplo:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00001-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
   ```
   {: screen}

4. Experimente seu aplicativo  ` Hello World ` .
   1. Obtenha o domínio padrão que está designado ao seu serviço Knative. Se você mudou o nome de seu serviço Knative ou implementou o app em um namespace diferente, atualize esses valores em sua consulta.
      ```
      kubectl get ksvc/kn-helloworld
      ```
      {: pre}

      Saída de exemplo:
      ```
      NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
      kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-rjmwt   kn-helloworld-rjmwt   True
      ```
      {: screen}

   2. Faça uma solicitação para seu app usando o subdomínio que você recuperou na etapa anterior.
      ```
      curl -v <service_domain>
      ```
      {: pre}

      Saída de exemplo:
      ```
      * URL reconstruída para: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
      *   Tentando 169.46.XX.XX ...
      * TCP_NODELAY set
      * Connected to kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud (169.46.XX.XX) port 80 (#0)
      > GET / HTTP/1.1
      > Host: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud
      > User-Agent: curl/7.54.0
      > Accept: */*
      >
      < HTTP/1.1 200 OK
      < Data: Thu, 21 Mar 2019 01:12:48 GMT
      < Content-Type: text/plain; charset=utf-8
      < Content-Length: 20
      Conexão <: keep-alive
      < x-enviado-upstream-service-time: 17
      <
      Hello de Amostra v1!
      * Connection #0 to host kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud left intact
      ```
      {: screen}

5. Aguarde alguns minutos para que o Knative diminua a capacidade de seu pod. O Knative avalia o número de pods que devem estar ativos em um momento para processar a carga de trabalho recebida. Se nenhum tráfego de rede for recebido, o Knative automaticamente diminuirá a capacidade de seus pods, mesmo até zero pods, conforme mostrado nesse exemplo.

   Deseja ver como o Knative aumentará a capacidade de seus pods? Tente aumentar a carga de trabalho para seu app, por exemplo, usando ferramentas como o [Testador de carga baseado em nuvem simples](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Se você não vir nenhum pods `kn-helloworld`, significa que o Knative diminuiu a capacidade do seu app para zero pods.

6. Atualize sua amostra de serviço Knative e insira um valor diferente para a variável de ambiente `TARGET`.

   Exemplo de serviço YAML:
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: kn-helloworld
     namespace: default
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           spec:
             container:
               image: docker.io/ibmcom/kn-helloworld
               env:
               - name: TARGET
                 value: "Mr. Smith"
    ```
    {: codeblock}

7. Aplique a mudança em seu serviço. Ao mudar a configuração, o Knative cria automaticamente uma nova revisão, designa uma nova rota e, por padrão, instrui o Istio a rotear o tráfego de rede recebido para a revisão mais recente.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

8. Faça uma nova solicitação ao seu aplicativo para verificar se a sua mudança foi aplicada.
   ```
   curl -v <service_domain>
   ```

   Saída de exemplo:
   ```
   ...
   Olá, Sr. Smith!
   ```
   {: screen}

9. Verifique se Knative aumentou o seu pod novamente para considerar o aumento do tráfego de rede. Seu pod recebe o número de revisão `00002`. É possível usar o número de revisão para referenciar uma versão específica de seu app, por exemplo, quando você deseja instruir o Istio para dividir o tráfego recebido entre as duas revisões.
   ```
   kubectl get pods
   ```

   Saída de exemplo:
   ```
   NAME                                              READY     STATUS    RESTARTS   AGE
   kn-helloworld-00002-deployment-55db6bf4c5-2vftm   3/3       Running   0          16s
   ```
   {: screen}

10. Opcional: limpe seu serviço Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}

Incrível! Você implementou com êxito seu primeiro app Knative em seu cluster e explorou os recursos de revisão e de ajuste de escala da primitiva `Serving` do Knative.


## O que Vem a Seguir?   
{: #whats-next}

- Experimente esse [Knative workshop ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM/knative101/tree/master/workshop) para implementar seu primeiro aplicativo fibonacci do `Node.js` em seu cluster.
  - Explore como usar a primitiva `Build` do Knative para construir uma imagem por meio de um Dockerfile no GitHub e enviar automaticamente a imagem para o seu namespace no {{site.data.keyword.registrylong_notm}}.  
  - Saiba como é possível configurar o roteamento para o tráfego de rede por meio do subdomínio do Ingress fornecido pela IBM para o gateway do Istio Ingress fornecido pelo Knative.
  - Apresente uma nova versão de seu app e use o Istio para controlar a quantia de tráfego que é roteada para cada versão do app.
- Explore amostras de `Eventing` do [Knative ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/knative/docs/tree/master/eventing/samples).
- Saiba mais sobre o Knative com a [documentação do Knative ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/knative/docs).
