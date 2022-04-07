---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Implementando apps serverless com Knative
{: #serverless-apps-knative}

Aprenda como instalar e usar o Knative em um cluster Kubernetes no {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

**O que é Knative e por que eu desejo usá-lo?**</br>
[Knative](https://github.com/knative/docs) é uma plataforma de software livre que foi desenvolvida pela IBM, Google, Pivotal, Red Hat, Cisco e outros. O objetivo é ampliar os recursos do Kubernetes para ajudar você a criar apps serverless e conteinerizados, modernos e centralizados na origem, na parte superior do cluster Kubernetes. A plataforma é projetada para atender às necessidades de desenvolvedores que hoje devem decidir qual tipo de aplicativo eles desejam executar na nuvem: apps de 12 fatores, contêineres ou funções. Cada tipo de aplicativo requer uma solução proprietária ou de software livre que esteja customizada para estes apps: Cloud Foundry para apps 12-factor, Kubernetes para contêineres e OpenWhisk e outros para funções. No passado, os desenvolvedores tinham que decidir qual abordagem desejavam seguir, o que levava à inflexibilidade e à complexidade quando diferentes tipos de apps tinham que ser combinados.  

O Knative usa uma abordagem consistente entre linguagens e estruturas de programação para abstrair a carga operacional de construção, implementação e gerenciamento de cargas de trabalho no Kubernetes para que os desenvolvedores possam se concentrar no que mais importa para eles: o código-fonte. É possível usar pacotes de construção comprovados com os que você já está familiarizado, como o Cloud Foundry, Kaniko, Dockerfile, Bazel e outros. Ao se integrar com o Istio, o Knative assegura que as cargas de trabalho sem servidor e conteinerizadas possam ser facilmente expostas na Internet, monitoradas e controladas e que seus dados sejam criptografados durante o trânsito.

**Como o Knative funciona?**</br>
O Knative é fornecido com três componentes principais ou _primitivas_, que ajudam a construir, implementar e gerenciar seus apps serverless em seu cluster Kubernetes:

- **Construção:** a primitiva `Build` suporta a criação de um conjunto de etapas para construir seu app por meio do código-fonte para uma imagem de contêiner. Imagine que você use um modelo de construção simples no qual especifica o repositório de origem para localizar seu código de app e o registro de contêiner no qual deseja hospedar a imagem. Com apenas um único comando, é possível instruir o Knative a adotar esse modelo de construção, fazer pull do código-fonte, criar a imagem e enviar por push a imagem para seu registro de contêiner para que seja possível usar a imagem em seu contêiner.
- **Servindo:** A primitiva `Serving` ajuda a implementar aplicativos sem servidor como serviços Knativos e para escaloná-los automaticamente, até mesmo para zero instâncias. Para expor suas cargas de trabalho serverless e conteinerizadas, o Knative usa o Istio. Ao instalar o complemento Knative gerenciado, o complemento Istio gerenciado é instalado automaticamente também. Usando o gerenciamento de tráfego e os recursos de roteamento inteligente do Istio, é possível controlar qual tráfego é roteado para uma versão específica de seu serviço, o que torna mais fácil para um desenvolvedor testar e apresentar uma nova versão do app ou executar um teste A-B.
- **Evento:** com a primitiva `Eventing`, é possível criar acionadores ou fluxos de eventos que outros serviços podem assinar. Por exemplo, você pode desejar iniciar uma nova construção de seu app sempre que o código for enviado por push para o repositório principal do GitHub. Ou você deseja executar um app sem servidor apenas se a temperatura cair abaixo do ponto de congelamento. Por exemplo, a primitiva `Eventing` pode ser integrada a seu pipeline CI/CD para automatizar a construção e a implementação de apps no caso de um evento específico ocorrer.

**O que é o complemento Knative gerenciado no {{site.data.keyword.containerlong_notm}} (experimental)?** </br>
O Knative gerenciado no {{site.data.keyword.containerlong_notm}} é um [complemento gerenciado](/docs/containers?topic=containers-managed-addons#managed-addons) que integra o Knative e o Istio diretamente ao cluster Kubernetes. As versões do Knative e do Istio no complemento são testadas pela IBM e suportadas para o uso no {{site.data.keyword.containerlong_notm}}. Para obter mais informações sobre os complementos gerenciados, consulte [Incluindo serviços por meio do uso de complementos gerenciados](/docs/containers?topic=containers-managed-addons#managed-addons).

**Há alguma limitação?** </br>
Se você instalou o [controlador de admissão de imposição de segurança da imagem de contêiner](/docs/services/Registry?topic=registry-security_enforce#security_enforce) em seu cluster, não será possível ativar o complemento Knative gerenciado no cluster.

## Configurando o Knative em seu cluster
{: #knative-setup}

O Knative é construído na parte superior do Istio para assegurar que as cargas de trabalho sem servidor e conteinerizadas possam ser expostas dentro do cluster e na Internet. Com o Istio, é possível também monitorar e controlar o tráfego de rede entre seus serviços e assegurar que seus dados sejam criptografados durante o trânsito. Ao instalar o complemento Knative gerenciado, o complemento Istio gerenciado é instalado automaticamente também.
{: shortdesc}

Antes de iniciar:
-  [Instale a CLI do IBM Cloud, o plug-in do {{site.data.keyword.containerlong_notm}} e a CLI do Kubernetes](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps). Certifique-se de instalar a versão da CLI `kubectl` que corresponde à versão do Kubernetes do seu cluster.
-  [Crie um cluster padrão com pelo menos 3 nós do trabalhador, cada um contendo 4 núcleos e 16 GB de memória (`b3c.4x16`) ou mais](/docs/containers?topic=containers-clusters#clusters_ui). Além disso, os nós do cluster e do trabalhador devem executar pelo menos a versão mínima suportada do Kubernetes, que pode ser revisada executando `ibmcloud ks addon-versions --addon knative`.
-  Assegure-se de que tenha a [função de serviço **Gravador** ou **Gerenciador** do IAM do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.containerlong_notm}}.
-  [Destino a CLI para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
</br>

Para instalar o Knative em seu cluster:

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

2. Verifique se o Istio foi instalado com êxito. Todos os pods para os nove serviços do Istio e o pod para o Prometheus devem estar em um status `Running`.
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

4. Verifique se todos os pods do componente Knative `Serving` estão em um estado `Em execução`.
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

## Usando serviços Knative para implementar um app serverless
{: #knative-deploy-app}

Depois de configurar o Knative em seu cluster, é possível implementar seu app serverless como um serviço Knative.
{: shortdesc}

**O que é um serviço Knative?**</br>
Para implementar um app com Knative, deve-se especificar um recurso `Service` Knative. Um serviço Knative é gerenciado pela primitiva `Serving` do Knative e é responsável por gerenciar o ciclo de vida inteiro da carga de trabalho. Quando você cria o serviço, a primitiva `Serving` do Knative cria automaticamente uma versão para seu app serverless e inclui essa versão no histórico de revisão do serviço. Seu app serverless é designado a uma URL pública por meio de seu subdomínio do Ingress no formato `<knative_service_name>.<namespace>.<ingress_subdomain>` que pode ser usado para acessar o aplicativo por meio da Internet. Além disso, um nome do host privado é designado ao seu app no formato `<knative_service_name>.<namespace>.cluster.local` que pode ser usado para acessar seu app de dentro do cluster.

**O que acontece nos bastidores quando eu crio o serviço Knative?**</br>
Quando você cria um serviço Knative, seu app é implementado automaticamente como um pod do Kubernetes em seu cluster e exposto usando um serviço do Kubernetes. Para designar o nome do host público, o Knative usa o subdomínio do Ingress fornecido pela IBM e o certificado TLS. O tráfego de rede recebido é roteado com base nas regras de roteamento do Ingress padrão fornecidas pela IBM.

**Como posso apresentar uma nova versão do meu app?**</br>
Ao atualizar seu serviço Knative, uma nova versão de seu app serverless é criada. Essa versão é designada aos mesmos nomes de host públicos e privados que sua versão anterior. Por padrão, todo o tráfego de rede recebido é roteado para a versão mais recente de seu app. No entanto, também é possível especificar a porcentagem de tráfego de rede recebido que você deseja rotear para uma versão específica do app para que seja possível executar o teste A-B. É possível dividir o tráfego de rede recebido entre duas versões de app de cada vez, a versão atual de seu app e a nova versão que você deseja substituir.  

**Posso trazer meu próprio domínio customizado e certificado TLS?**</br>
É possível mudar o configmap de seu gateway do Ingress do Istio e as regras de roteamento do Ingress para usar seu nome de domínio customizado e certificado TLS ao designar um nome do host a seu app serverless. Para obter mais informações, consulte [Configurando nomes de domínio e certificados customizados](#knative-custom-domain-tls).

Para implementar seu app serverless como um serviço Knative:

1. Crie um arquivo YAML para seu primeiro app [`Hello World`](https://hub.docker.com/r/ibmcom/kn-helloworld) serverless em Go com Knative. Ao enviar uma solicitação para seu app de amostra, ele lê a variável de ambiente `TARGET` e imprime `"Hello ${TARGET}!"`. Se esta variável de ambiente estiver vazia, `"Hello World!"` será retornado.
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
    <td>Opcional: o namespace do Kubernetes no qual você deseja implementar seu app como um serviço Knative. Por padrão, todos os serviços são implementados no namespace <code>default</code> do Kubernetes. </td>
    </tr>
    <tr>
    <td><code> spec.container.image </code></td>
    <td>A URL para o registro do contêiner no qual sua imagem está armazenada. Neste exemplo, você implementa um app Hello World do Knative que está armazenado no namespace <code>ibmcom</code> no Docker Hub. </td>
    </tr>
    <tr>
    <td><code> spec.container.env </code></td>
    <td>Opcional: uma lista de variáveis de ambiente que você deseja que seu serviço Knative tenha. Neste exemplo, o valor da variável de ambiente <code>TARGET</code> é lido pelo aplicativo de amostra e retornado quando você envia uma solicitação para seu app no formato <code>"Hello ${TARGET}!"</code>. Se nenhum valor for fornecido, o app de amostra retornará <code>"Hello World!"</code>.  </td>
    </tr>
    </tbody>
    </table>

2. Crie o serviço Knative em seu cluster.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

   Saída de exemplo:
   ```
   service.serving.knative.dev "kn-helloworld" criado
   ```
   {: screen}

3. Verifique se o serviço Knative foi criado. Na saída da CLI, é possível visualizar o **DOMAIN** público que está designado ao seu app serverless. As colunas **LATESTCREATED** e **LATESTREADY** mostram a versão de seu app que foi criada pela última vez e que está atualmente implementada no formato `<knative_service_name>-<version>`. A versão que é designada ao seu app é um valor de sequência aleatória. Neste exemplo, a versão de seu app serverless é `rjmwt`. Quando você atualiza o serviço, uma nova versão de seu app é criada e atribuída a uma nova sequência aleatória para a versão.  
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

4. Experimente seu app `Hello World` enviando uma solicitação para a URL pública que está designada ao seu app.
   ```
   curl -v <public_app_url>
   ```
   {: pre}

   Saída de exemplo:
   ```
   * Rebuilt URL to: kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud/
   *   Trying 169.46.XX.XX...
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

5. Liste o número de pods que foram criados para seu serviço Knative. No exemplo neste tópico, um pod que consiste em dois contêineres é implementado. Um contêiner executa seu app `Hello World` e o outro contêiner é um sidecar que executa as ferramentas de monitoramento e criação de log do Istio e do Knative.
   ```
   kubectl get pods
   ```
   {: pre}

   Saída de exemplo:
   ```
   NAME                                             READY     STATUS    RESTARTS   AGE
   kn-helloworld-rjmwt-deployment-55db6bf4c5-2vftm  2/2      Running   0          16s
   ```
   {: screen}

6. Aguarde alguns minutos para que o Knative diminua a capacidade de seu pod. O Knative avalia o número de pods que devem estar ativos em um momento para processar a carga de trabalho recebida. Se nenhum tráfego de rede for recebido, o Knative automaticamente diminuirá a capacidade de seus pods, mesmo até zero pods, conforme mostrado nesse exemplo.

   Deseja ver como o Knative aumentará a capacidade de seus pods? Tente aumentar a carga de trabalho para seu app, por exemplo, usando ferramentas como o [Testador de carga baseado em nuvem simples](https://loader.io/).
   {: tip}
   ```
   kubectl get pods
   ```
   {: pre}

   Se você não vir nenhum pods `kn-helloworld`, significa que o Knative diminuiu a capacidade do seu app para zero pods.

7. Atualize sua amostra de serviço Knative e insira um valor diferente para a variável de ambiente `TARGET`.

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

8. Aplique a mudança em seu serviço. Quando você muda a configuração, o Knative cria automaticamente uma nova versão para seu app.
   ```
   kubectl apply -f service.yaml
   ```
   {: pre}

9. Verifique se uma nova versão de seu app está implementada. Na saída da CLI, é possível ver a nova versão de seu app na coluna **LATESTCREATED**. Quando você vê a mesma versão do app na coluna **LATESTREADY**, seu app está todo configurado e pronto para receber o tráfego de rede recebido na URL pública designada.
   ```
   kubectl get ksvc/kn-helloworld
   ```
   {: pre}

   Saída de exemplo:
   ```
   NAME            DOMAIN                                                                LATESTCREATED         LATESTREADY           READY   REASON
   kn-helloworld   kn-helloworld.default.mycluster.us-south.containers.appdomain.cloud   kn-helloworld-ghyei   kn-helloworld-ghyei   True
   ```
   {: screen}

9. Faça uma nova solicitação ao seu aplicativo para verificar se a sua mudança foi aplicada.
   ```
   curl -v <service_domain>
   ```

   Saída de exemplo:
   ```
   ...
   Olá, Sr. Smith!
   ```
   {: screen}

10. Verifique se Knative aumentou o seu pod novamente para considerar o aumento do tráfego de rede.
    ```
    kubectl get pods
    ```

    Saída de exemplo:
    ```
    NAME                                              READY     STATUS    RESTARTS   AGE
    kn-helloworld-ghyei-deployment-55db6bf4c5-2vftm   2/2       Running   0          16s
    ```
    {: screen}

11. Opcional: limpe seu serviço Knative.
    ```
    kubectl delete -f service.yaml
    ```
    {: pre}


## Configurando nomes de domínio e certificados customizados
{: #knative-custom-domain-tls}

É possível configurar o Knative para designar nomes de host de seu próprio domínio customizado que você configurou com TLS.
{: shortdesc}

Por padrão, cada app é designado a um subdomínio público de seu subdomínio do Ingress no formato `<knative_service_name>.<namespace>.<ingress_subdomain>` que pode ser usado para acessar o app por meio da Internet. Além disso, um nome do host privado é designado ao seu app no formato `<knative_service_name>.<namespace>.cluster.local` que pode ser usado para acessar seu app de dentro do cluster. Se deseja designar nomes de host de um domínio customizado que você possui, é possível mudar o configmap do Knative para usar o domínio customizado no lugar.

1. Crie um domínio customizado. Para registrar seu domínio customizado, trabalhe com seu provedor Domain Name Service (DNS) ou [IBM Cloud DNS](/docs/infrastructure/dns?topic=dns-getting-started).
2. Configure seu domínio para rotear o tráfego de rede recebido para o gateway do Ingress fornecido pela IBM. Escolha entre estas opções:
   - Defina um alias para seu domínio customizado especificando o domínio fornecido pela IBM como um registro de Nome Canônico (CNAME). Para localizar o domínio do Ingress fornecido pela IBM, execute `ibmcloud ks cluster-get --cluster <cluster_name>` e procure o campo **Subdomínio do Ingress**. O uso de um CNAME é preferencial porque a IBM fornece verificações de funcionamento automáticas no subdomínio IBM e remove os IPs com falha da resposta de DNS.
   - Mapeie seu domínio customizado para o endereço IP público móvel do gateway do Ingress incluindo o endereço IP como um registro. Para localizar o endereço IP público do gateway do Ingress, execute `nslookup <ingress_subdomain>`.
3. Compre um certificado TLS curinga oficial para seu domínio customizado. Se você desejar comprar múltiplos certificados TLS, certifique-se de que o [CN ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://support.dnsimple.com/articles/what-is-common-name/) seja diferente para cada certificado.
4. Crie um segredo do Kubernetes para seu certificado e chave.
   1. Codifique o certificado e a chave na base 64 e salve o valor codificado na base 64 em um novo arquivo.
      ```
      openssl base64 -in tls.key -out tls.key.base64
      ```
      {: pre}

   2. Visualize o valor codificado com base 64 para seu certificado e chave.
      ```
      cat tls.key.base64
      ```
      {: pre}

   3. Crie um arquivo YAML secreto usando o certificado e a chave.
      ```
      apiVersion: v1
      kind: Secret
      metadata:
        name: mydomain-ssl
      type: Opaque
      data:
        tls.crt: <client_certificate>
        tls.key: <client_key>
      ```
      {: codeblock}

   4. Crie o certificado em seu cluster.
      ```
      kubectl create -f secret.yaml
      ```
      {: pre}

5. Abra o recurso do Ingress `iks-knative-ingress` no namespace `istio-system` de seu cluster para começar a editá-lo.
   ```
   kubectl edit ingress iks-knative-ingress -n istio-system
   ```
   {: pre}

6. Mude as regras de roteamento padrão para seu Ingress.
   - Inclua seu domínio curinga customizado na seção `spec.rules.host` para que todo o tráfego de rede recebido de seu domínio customizado e qualquer subdomínio seja roteado para o `istio-ingressgateway`.
   - Configure todos os hosts de seu domínio curinga customizado para usar o segredo do TLS que você criou anteriormente na seção `spec.tls.hosts`.

   Ingress de exemplo:
   ```
   apiVersion: extensions/v1beta1
   kind: Ingress
   metadata:
     name: iks-knative-ingress
     namespace: istio-system
   spec:
     rules:
     - host: '*.mydomain'
       http:
         paths:
         - backend:
             serviceName: istio-ingressgateway
             servicePort: 80
           path: /
     tls:
     - hosts:
       - '*.mydomain'
       secretName: mydomain-ssl
   ```
   {: codeblock}

   As seções `spec.rules.host` e `spec.tls.hosts` são listas e podem incluir múltiplos domínios customizados e certificados TLS.
   {: tip}

7. Modifique o configmap `config-domain` do Knative para usar seu domínio customizado para designar nomes de host a novos serviços Knative.
   1. Abra o configmap `config-domain` para começar a editá-lo.
      ```
      kubectl edit configmap config-domain -n knative-serving
      ```
      {: pre}

   2. Especifique seu domínio customizado na seção `data` de seu configmap e remova o domínio padrão que está configurado para seu cluster.
      - **Exemplo para designar um nome do host de seu domínio customizado para todos os serviços Knative**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: ""
        metadata: name: config-domain namespace: knative-serving
        ```
        {: codeblock}

        Incluindo `""` em seu domínio customizado, todos os serviços Knative criados são designados a um nome do host de seu domínio customizado.  

      - **Exemplo para designar um nome do host de seu domínio customizado para selecionar serviços Knative**:
        ```
        apiVersion: v1
        kind: ConfigMap
        data:
          <custom_domain>: |
            selector:
              app: sample
          mycluster.us-south.containers.appdomain.cloud: ""
        metadata: name: config-domain namespace: knative-serving
        ```
        {: codeblock}

        Para designar um nome do host de seu domínio customizado para selecionar somente serviços Knative, inclua uma chave e um valor de rótulo `data.selector` em seu configmap. Neste exemplo, todos os serviços com o rótulo `app: sample` são designados a um nome do host de seu domínio customizado. Certifique-se de que também tenha um nome de domínio que você deseja designar a todos os outros apps que não têm o rótulo `app: sample`. Neste exemplo, o domínio fornecido pela IBM padrão `mycluster.us-south.containers.appdomain.cloud` é usado.
    3. Salve as suas mudanças.

Com suas regras de roteamento do Ingress e os configmaps do Knative todos configurados, é possível criar serviços Knative com seu domínio customizado e certificado TLS.

## Acessando um serviço Knative por meio de outro serviço Knative
{: #knative-access-service}

É possível acessar seu serviço Knative por meio de outro serviço Knative usando uma chamada da API de REST para a URL que é designada ao seu serviço Knative.
{: shortdesc}

1. Liste todos os serviços Knative em seu cluster.
   ```
   kubectl get ksvc --all-namespaces
   ```
   {: pre}

2. Recupere o **DOMAIN** que está designado ao seu serviço Knative.
   ```
   kubect get ksvc/<knative_service_name>
   ```
   {: pre}

   Saída de exemplo:
   ```
   NAME        DOMAIN                                                            LATESTCREATED     LATESTREADY       READY   REASON
   myservice   myservice.default.mycluster.us-south.containers.appdomain.cloud   myservice-rjmwt   myservice-rjmwt   True
   ```
   {: screen}

3. Use o nome do domínio para implementar uma chamada da API de REST para acessar seu serviço Knative. Essa chamada da API de REST deve ser parte do app para o qual você cria um serviço Knative. Se o serviço Knative que você deseja acessar for designado a uma URL local no formato `<service_name>.<namespace>.svc.cluster.local`, o Knative manterá a solicitação da API de REST dentro da rede interna do cluster.

   Fragmento de código de exemplo em Go:
   ```go
   resp, err := http.Get("<knative_service_domain_name>")
   if err != nil {
       fmt.Fprintf(os.Stderr, "Error: %s\n", err)
   } else if resp.Body != nil {
       body, _ := ioutil.ReadAll(resp.Body)
       fmt.Printf("Response: %s\n", string(body))
   }
   ```
   {: codeblock}

## Configurações comuns do serviço Knative
{: #knative-service-settings}

Revise as configurações comuns do serviço Knative que você pode achar úteis à medida que desenvolve seu app serverless.
{: shortdesc}

- [Configurando os números mínimo e máximo de pods](#knative-min-max-pods)
- [Especificando o número máximo de solicitações por pod](#max-request-per-pod)
- [Criando apps serverless somente privados](#knative-private-only)
- [Forçando o serviço Knative a refazer pull de uma imagem de contêiner](#knative-repull-image)

### Configurando os números mínimo e máximo de pods
{: #knative-min-max-pods}

É possível especificar os números mínimo e máximo de pods que você deseja executar para seus apps usando uma anotação. Por exemplo, se você não desejar que o Knative reduza a escala de seu app para nenhuma instância, configure o número mínimo de pods como 1.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: ...
spec:
  runLatest:
    configuration:
      revisionTemplate:
        metadata:
          annotations:
            autoscaling.knative.dev/minScale: "1"
            autoscaling.knative.dev/maxScale: "100"
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Entendendo os componentes de arquivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
</thead>
<tbody>
<tr>
<td><code>autoscaling.knative.dev/minScale</code></td>
<td>Insira o número mínimo de pods que você deseja executar em seu cluster. O Knative não poderá reduzir a escala do seu app para um número menor do que você configurou, mesmo se nenhum tráfego de rede for recebido por seu app. O número padrão de pods é zero.  </td>
</tr>
<tr>
<td><code>autoscaling.knative.dev/maxScale</code></td>
<td>Insira o número máximo de pods que você deseja executar em seu cluster. O Knative não poderá aumentar a escala do seu app para um número mais alto do que você configurou, mesmo se você tiver mais solicitações que suas instâncias de app atuais podem manipular.</td>
</tr>
</tbody>
</table>

### Especificando o número máximo de solicitações por pod
{: #max-request-per-pod}

É possível especificar o número máximo de solicitações que uma instância do app pode receber e processar antes que o Knative considere aumentar a escala das instâncias do app. Por exemplo, se você configurar o número máximo de solicitações para 1, sua instância do app poderá receber uma solicitação por vez. Se uma segunda solicitação chegar antes de a primeira ser totalmente processada, o Knative aumentará a escala de outra instância.

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: myimage
          containerConcurrency: 1
....
```
{: codeblock}

<table>
<caption>Entendendo os componentes de arquivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
</thead>
<tbody>
<tr>
<td><code>containerConcurrency</code></td>
<td>Insira o número máximo de solicitações que uma instância do app pode receber de cada vez antes que o Knative considere aumentar a escala de suas instâncias do app.  </td>
</tr>
</tbody>
</table>

### Criando apps serverless somente privados
{: #knative-private-only}

Por padrão, cada serviço Knative é designado a uma rota pública de seu subdomínio do Ingress do Istio e uma rota privada no formato `<service_name>.<namespace>.cluster.local`. É possível usar a rota pública para acessar seu app por meio da rede pública. Se você deseja manter seu serviço privado, é possível incluir o rótulo `serving.knative.dev/visibility` em seu serviço Knative. Esse rótulo instrui o Knative a designar somente um nome do host privado ao seu serviço.
{: shortdesc}

```
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: myservice
  labels:
    serving.knative.dev/visibility: cluster-local
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image:
...
```
{: codeblock}

<table>
<caption>Entendendo os componentes de arquivo YAML</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
</thead>
<tbody>
<tr>
<td><code>serving.knative.dev/visibility</code></td>
  <td>Se você incluir o rótulo <code>serving.knative.dev/visibility: cluster-local</code>, seu serviço será designado somente a uma rota privada no formato <code>&lt;service_name&gt;.&lt;namespace&gt;.cluster.local</code>. É possível usar o nome do host privado para acessar seu serviço de dentro do cluster, mas não é possível acessar seu serviço por meio da rede pública.  </td>
</tr>
</tbody>
</table>

### Forçando o serviço Knative a refazer pull de uma imagem de contêiner
{: #knative-repull-image}

A implementação atual de Knative não fornece uma maneira padrão de forçar o componente `Serving` do Knative a fazer novamente pull de uma imagem de contêiner. Para fazer novamente pull de uma imagem de seu registro, escolha entre as opções a seguir:

- **Modifique o serviço Knative `revisionTemplate`**: o `revisionTemplate` de um serviço Knative é usado para criar uma revisão de seu serviço Knative. Se você modificar esse modelo de revisão e, por exemplo, incluir a anotação `repullFlag`, o Knative deverá criar uma nova revisão para seu app. Como parte da criação da revisão, o Knative deve verificar as atualizações de imagem de contêiner. Quando você configura `imagePullPolicy: Always`, o Knative não pode usar o cache de imagem no cluster, mas, em vez disso, deve fazer pull da imagem do registro de contêiner.
   ```
   apiVersion: serving.knative.dev/v1alpha1
   kind: Service
   metadata:
     name: <service_name>
   spec:
     runLatest:
       configuration:
         revisionTemplate:
           metadata:
             annotations:
               repullFlag: 123
           spec:
             container:
               image: <image_name>
               imagePullPolicy: Always
    ```
    {: codeblock}

    Deve-se mudar o valor `repullFlag` toda vez que você deseja criar uma nova revisão de seu serviço que faz pull da versão da imagem mais recente do registro de contêiner. Certifique-se de usar um valor exclusivo para cada revisão para evitar que o Knative use uma versão de imagem antiga devido a duas configurações de serviço Knative idênticas.  
    {: note}

- **Usar tags para criar imagens de contêiner exclusivas**: é possível usar tags exclusivas para cada imagem de contêiner que você cria e referenciar essa imagem em sua configuração `container.image` do serviço Knative. No exemplo a seguir, `v1` é usado como a tag de imagem. Para forçar o Knative a fazer pull de uma nova imagem de seu registro de contêiner, deve-se mudar a tag de imagem. Por exemplo, use `v2` como sua nova tag de imagem.
  ```
  apiVersion: serving.knative.dev/v1alpha1
  kind: Service
  metadata:
    name: <service_name>
  spec:
    runLatest:
      configuration:
          spec:
            container:
              image: myapp:v1
              imagePullPolicy: Always
    ```
    {: codeblock}


## Links relacionados  
{: #knative-related-links}

- Experimente esse [Knative workshop ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM/knative101/tree/master/workshop) para implementar seu primeiro aplicativo fibonacci do `Node.js` em seu cluster.
  - Explore como usar a primitiva `Build` do Knative para construir uma imagem por meio de um Dockerfile no GitHub e enviar automaticamente a imagem para o seu namespace no {{site.data.keyword.registrylong_notm}}.  
  - Saiba como é possível configurar o roteamento para o tráfego de rede por meio do subdomínio do Ingress fornecido pela IBM para o gateway do Istio Ingress fornecido pelo Knative.
  - Apresente uma nova versão de seu app e use o Istio para controlar a quantia de tráfego que é roteada para cada versão do app.
- Explore amostras de `Eventing` do [Knative ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/knative/docs/tree/master/eventing/samples).
- Saiba mais sobre o Knative com a [documentação do Knative ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/knative/docs).
