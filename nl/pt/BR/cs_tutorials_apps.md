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


# Tutorial: Implementando apps em clusters do Kubernetes
{: #cs_apps_tutorial}

É possível aprender como usar o {{site.data.keyword.containerlong}} para implementar um app conteinerizado que alavanca o {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

Neste cenário, uma firma PR fictícia usa o serviço {{site.data.keyword.Bluemix_notm}} para analisar seus press releases e receber feedback no sinal de suas mensagens.

Usando o cluster do Kubernetes que foi criado no último tutorial, o desenvolvedor de app da firma PR implementa uma versão Hello World do app. Construindo em cada lição neste tutorial, o desenvolvedor de aplicativo implementa versões progressivamente mais complicadas do mesmo app. O diagrama a seguir mostra os componentes de cada implementação por lição.

<img src="images/cs_app_tutorial_mz-roadmap.png" width="700" alt="Lesson components" style="width:700px; border-style: none"/>

Conforme descrito no diagrama, o Kubernetes usa vários tipos diferentes de recursos para deixar seus apps funcionando em clusters. No Kubernetes, as implementações e os serviços funcionam juntos. As implementações incluem as definições para o app. Por exemplo, a imagem a ser usada para o contêiner e qual porta deve ser exposta para o app. Ao criar uma implementação, um pod do Kubernetes é criado para cada contêiner definido na implementação. Para tornar seu app mais resiliente, é possível definir múltiplas instâncias do mesmo app em sua implementação e permitir que o Kubernetes crie automaticamente um conjunto de réplicas para você. O conjunto de réplicas monitora os pods e assegura que o número especificado de pods estejam sempre funcionando. Se um dos pods tornar-se não responsivo, o pod será recriado automaticamente.

Os serviços agrupam um conjunto de cápsulas e fornecem conexão de rede a esses pods para outros serviços no cluster sem expor o endereço IP privado real de cada pod. É possível usar os serviços do Kubernetes para tornar um app disponível para outros pods dentro do cluster ou para expor um app na Internet. Neste tutorial, você usa um serviço do Kubernetes para acessar seu app em execução na Internet usando um endereço IP público que é designado automaticamente a um nó do trabalhador e uma porta pública.

Para tornar seu app ainda mais altamente disponível, em clusters padrão, é possível criar um conjunto de trabalhadores que abrange múltiplas zonas com nós do trabalhador em cada zona para executar ainda mais réplicas de seu app. Essa tarefa não é coberta neste tutorial, mas mantenha esse conceito em mente para melhorias futuras na disponibilidade de um app.

Somente uma das lições inclui a integração de um serviço do {{site.data.keyword.Bluemix_notm}} em um app, mas é possível usá-las com um app tão simples ou complexo quanto se possa imaginar.

## Objetivos
{: #apps_objectives}

* Entender a terminologia básica do Kubernetes
* Enviar por push uma imagem para o namespace de seu registro no {{site.data.keyword.registryshort_notm}}
* Tornar um app publicamente acessível
* Implementar uma única instância de um app em um cluster usando um comando do Kubernetes e um script
* Implementar múltiplas instâncias de um app em contêineres que são recriados durante as verificações de funcionamento
* Implementar um app que use a funcionalidade de um serviço do {{site.data.keyword.Bluemix_notm}}

## Tempo Necessário
{: #apps_time}

40
minutos

## Públicos
{: #apps_audience}

Os desenvolvedores de software e administradores da rede que estão implementando um app em um cluster do Kubernetes pela primeira vez.

## Pré-requisitos
{: #apps_prereqs}

[Tutorial: criando clusters do Kubernetes](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)


## Lição 1: implementando apps de instância única em clusters do Kubernetes
{: #cs_apps_tutorial_lesson1}

No tutorial anterior, você criou um cluster com um nó do trabalhador. Nesta lição, você configura uma implementação e implementa uma instância única do app em um pod do Kubernetes no nó do trabalhador.
{:shortdesc}

Os componentes que você implementa concluindo esta lição são mostrados no diagrama a seguir.

![Configuração de implementação](images/cs_app_tutorial_mz-components1.png)

Para implementar o app:

1.  Clone o código-fonte para o [app Hello World ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM/container-service-getting-started-wt) para o diretório inicial do usuário. O repositório contém versões diferentes de um app semelhante em pastas iniciadas com `Lab`. Cada versão contém os arquivos a seguir:
    * `Dockerfile`: as definições de construção para a imagem.
    * `app.js`: o app Hello World.
    * `package.json`: metadados sobre o app.

    ```
    git clone https://github.com/IBM/container-service-getting-started-wt.git
    ```
    {: pre}

2.  Navegue para o diretório `Lab 1`.

    ```
    cd 'container-service-getting-started-wt/Lab 1'
    ```
    {: pre}

3. [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

5.  Efetue login na CLI do {{site.data.keyword.registryshort_notm}}.

    ```
    ibmcloud cr login
    ```
    {: pre}
    -   Se você esqueceu o seu namespace no {{site.data.keyword.registryshort_notm}}, execute o comando a seguir.

        ```
        ibmcloud cr namespace-list
        ```
        {: pre}

6.  Construa uma imagem do Docker que inclua os arquivos de app do diretório `Lab 1` e envie por push a imagem para o namespace do {{site.data.keyword.registryshort_notm}} que você criou no tutorial anterior. Caso seja necessário fazer uma mudança no app no futuro, repita estas etapas para criar outra versão da imagem. **Nota**: saiba mais sobre [proteção de suas informações pessoais](/docs/containers?topic=containers-security#pi) quando você trabalhar com imagens de contêiner.

    Use caracteres alfanuméricos minúsculos ou sublinhados (`_`) somente no nome da imagem. Não esqueça o ponto (`.`) no final do comando. O ponto indica
ao Docker para verificar dentro do diretório atual para o Dockerfile e construir artefatos para construir a
imagem. Para obter a região de registro em que você está atualmente, execute `ibmcloud cr region`.

    ```
    ibmcloud cr build -t <region>.icr.io/<namespace>/hello-world:1 .
    ```
    {: pre}

    Quando a construção estiver completa, verifique se você vê a mensagem de êxito a seguir:

    ```
    Successfully built <image_ID> Successfully tagged <region>.icr.io/<namespace>/hello-world:1 The push refers to a repository [<region>.icr.io/<namespace>/hello-world] 29042bc0b00c: Pushed f31d9ee9db57: Pushed 33c64488a635: Pushed 0804854a4553: Layer already exists 6bd4a62f5178: Layer already exists 9dfa40a0da3b: Layer already exists 1: digest: sha256:f824e99435a29e55c25eea2ffcbb84be4b01345e0a3efbd7d9f238880d63d4a5 size: 1576
    ```
    {: screen}

7.  As implementações são usadas para gerenciar pods, que incluem instâncias conteinerizadas de um app. O comando a seguir implementa o app em um único pod. Para os propósitos deste tutorial, a implementação é denominada **hello-world-deployment**, mas é possível fornecer à implementação qualquer nome que você desejar.

    ```
    kubectl create deployment hello-world-deployment --image=<region>.icr.io/<namespace>/hello-world:1
    ```
    {: pre}

    Saída de exemplo:

    ```
    deployment "hello-world-deployment" created
    ```
    {: screen}

    Saiba mais sobre [como proteger suas informações pessoais](/docs/containers?topic=containers-security#pi) quando trabalhar com recursos do Kubernetes.

8.  Torne o app acessível ao mundo expondo a implementação como um serviço NodePort. Assim como você pode expor uma porta para um app Cloud Foundry, o NodePort que você expõe é a porta na qual o nó do trabalhador atende o tráfego.

    ```
    kubectl expose deployment/hello-world-deployment --type=NodePort --port=8080 --name=hello-world-service --target-port=8080
    ```
    {: pre}

    Saída de exemplo:

    ```
    service "hello-world-service" exposed
    ```
    {: screen}

    <table summary=“Information about the expose command parameters.”>
    <caption>Mais sobre os parâmetros de exposição</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> Mais sobre os parâmetros de exposição</th>
    </thead>
    <tbody>
    <tr>
    <td><code>expose</code></td>
    <td>Exponha um recurso como um serviço do Kubernetes e disponibilize-o publicamente para os usuários.</td>
    </tr>
    <tr>
    <td><code>deployment/<em>&lt;hello-world-deployment&gt;</em></code></td>
    <td>O tipo de recurso e o nome do recurso a serem expostos com este serviço.</td>
    </tr>
    <tr>
    <td><code>--name=<em>&lt;hello-world-service&gt;</em></code></td>
    <td>O nome do serviço.</td>
    </tr>
    <tr>
    <td><code>--port=<em>&lt;8080&gt;</em></code></td>
    <td>A porta na qual o serviço atende.</td>
    </tr>
    <tr>
    <td><code>--type=NodePort</code></td>
    <td>O tipo de serviço a ser criado.</td>
    </tr>
    <tr>
    <td><code>--target-port=<em>&lt;8080&gt;</em></code></td>
    <td>A porta para a qual o serviço direciona o tráfego. Nessa instância, a porta de destino é a mesma que a porta, mas outros apps que você criar poderão diferir.</td>
    </tr>
    </tbody></table>

9. Agora que todo o trabalho de implementação está pronto, é possível testar seu app em um navegador. Obtenha os detalhes para formar a URL.
    1.  Obtenha informações sobre o serviço para ver qual NodePort foi designado.

        ```
        Kubectl describe service hello-world-service
        ```
        {: pre}

        Saída de exemplo:

        ```
        Name:                   hello-world-service
        Namespace:              default
        Labels:                 run=hello-world-deployment
        Selector:               run=hello-world-deployment
        Type:                   NodePort
        IP:                     10.xxx.xx.xxx
        Port:                   <unset> 8080/TCP
        NodePort:               <unset> 30872/TCP
        Endpoints:              172.30.xxx.xxx:8080
        Session Affinity:       None
        No events.
        ```
        {: screen}

        Os NodePorts são designados aleatoriamente quando são gerados com o comando `expose`, mas dentro de 30000-32767. Neste exemplo, o NodePort é 30872.

    2.  Obtenha o endereço IP público para o nó do trabalhador no cluster.

        ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Saída de exemplo:

        ```
        ibmcloud ks workers --cluster pr_firm_cluster
        Listing cluster workers...
        OK
        ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
        kube-mil01-pa10c8f571c84d4ac3b52acbf50fd11788-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.13.6
        ```
        {: screen}

10. Abra um navegador e consulte o aplicativo com a URL a seguir: `http://<IP_address>:<NodePort>`. Com os valores de exemplo, a URL é `http://169.xx.xxx.xxx:30872`. Ao inserir essa URL em um navegador, é possível ver o texto a seguir.

    ```
    Hello world! Seu app está funcionando em um contêiner!
    ```
    {: screen}

    Para ver se o app está publicamente disponível, tente inseri-lo em um navegador em seu telefone celular.
    {: tip}

11. [ Ativar o painel do Kubernetes ](/docs/containers?topic=containers-app#cli_dashboard).

    Se você selecionar seu cluster no [console do {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/), será possível usar o botão **Painel do Kubernetes** para ativar seu painel com um clique.
    {: tip}

12. Na guia **Cargas de trabalho**, é possível ver os recursos que você criou.

Bom trabalho! Você implementou sua primeira versão do app.

Muitos comandos nesta lição? Acordado. Que tal usar um script de configuração para fazer alguns dos trabalhos para você? Para usar um script de configuração para a segunda versão do app e para criar maior disponibilidade implementando múltiplas instâncias desse app, continue com a próxima lição.

<br />


## Lição 2: implementando e atualizando apps com disponibilidade mais alta
{: #cs_apps_tutorial_lesson2}

Nessa lição, você implementa três instâncias do app Hello World em um cluster para obter maior disponibilidade que a primeira versão do app.
{:shortdesc}

Disponibilidade mais alta significa que o acesso de usuário é dividido entre as três instâncias. Quando muitos usuários estão tentando acessar a mesma instância do app, eles podem observar tempos de resposta lentos. Múltiplas instâncias podem significar tempos de resposta mais rápidos para seus usuários. Nesta lição, você também aprenderá como as verificações de funcionamento e atualizações de implementação podem funcionar com o Kubernetes. O diagrama a seguir inclui os componentes que você implementa concluindo esta lição.

![Configuração de implementação](images/cs_app_tutorial_mz-components2.png)

No tutorial anterior, você criou sua conta e um cluster com um nó do trabalhador. Nesta lição, você configura uma implementação e implementa três instâncias do app Hello World. Cada instância é implementada em um pod do Kubernetes como parte de um conjunto de réplicas no nó do trabalhador. Para torná-la publicamente disponível, você também cria um serviço do Kubernetes.

Conforme definido no script de configuração, o Kubernetes pode usar uma verificação de disponibilidade para ver se um contêiner em um pod está em execução ou não. Por exemplo, essas verificações podem capturar conflitos, em que um app está em execução, mas não consegue progredir. Reiniciar um contêiner que está nessa condição pode ajudar a tornar o app mais disponível apesar de erros. Então, o Kubernetes usa uma verificação de prontidão para saber quando um contêiner está pronto para começar a aceitar o tráfego novamente. Um pod é considerado pronto quando seu contêiner está pronto. Quando o pod está pronto, ele é iniciado novamente. Nesta versão do app, a cada 15 segundos, ele atinge o tempo limite. Com uma verificação de funcionamento configurada no script de configuração, os contêineres serão recriados se a verificação de funcionamento localizar um problema com um app.

1.  Em uma CLI, navegue para o diretório `Lab 2`.

  ```
  cd 'container-service-getting-started-wt/Lab 2'
  ```
  {: pre}

2.  Se tiver iniciado uma nova sessão da CLI, efetue login e configure o contexto do cluster.

3.  Construa, identifique e envie por push o app como uma imagem para seu namespace no {{site.data.keyword.registryshort_notm}}.  Novamente, não esqueça o ponto (`.`) no final do comando.

    ```
    ibmcloud cr build -t <region>.icr.io/<namespace>/hello-world:2 .
      ```
    {: pre}

    Verifique se você vê a mensagem de êxito.

    ```
    Successfully built <image_ID> Successfully tagged <region>.icr.io/<namespace>/hello-world:1 The push refers to a repository [<region>.icr.io/<namespace>/hello-world] 29042bc0b00c: Pushed f31d9ee9db57: Pushed 33c64488a635: Pushed 0804854a4553: Layer already exists 6bd4a62f5178: Layer already exists 9dfa40a0da3b: Layer already exists 1: digest: sha256:f824e99435a29e55c25eea2ffcbb84be4b01345e0a3efbd7d9f238880d63d4a5 size: 1576
    ```
    {: screen}

4.  Abra o arquivo `healthcheck.yml`, no diretório `Lab 2`, com um editor de texto. Esse script de configuração combina algumas etapas da lição anterior para criar uma implementação e um serviço ao mesmo tempo. Os desenvolvedores de app da firma PR podem usar esses scripts quando atualizações são feitas ou para solucionar problemas ao recriar os pods.
    1. Atualize os detalhes para a imagem em seu namespace de registro privado.

        ```
        image: "<region>.icr.io/<namespace>/hello-world:2"
        ```
        {: codeblock}

    2.  Na seção **Implementação**, observe as `replicas`. As réplicas
são o número de instâncias de seu app. Executar três instâncias torna o app mais altamente disponível
do que apenas uma instância.

        ```
        replicas: 3
        ```
        {: codeblock}

    3.  Observe a análise de vivacidade de HTTP que verifica o funcionamento do contêiner a cada 5 segundos.

        ```
        livenessProbe:
                    httpGet:
                      path: /healthz
                      port: 8080
                    initialDelaySeconds: 5
                    periodSeconds: 5
        ```
        {: codeblock}

    4.  Na seção **Serviço**, observe o `NodePort`. Em vez de gerar um NodePort aleatório como você fez na lição anterior, é possível especificar uma porta no intervalo 30000 - 32767. Esse exemplo usa 30072.

5.  Altere novamente para a CLI que foi usada para configurar o contexto do cluster e execute o script de configuração. Quando a implementação e o serviço são criados, o app fica disponível para os usuários da firma PR verem.

  ```
  kubectl apply -f healthcheck.yml
  ```
  {: pre}

  Saída de exemplo:

  ```
  deployment "hw-demo-deployment" created
  service "hw-demo-service" created
  ```
  {: screen}

6.  Agora que o trabalho de implementação está pronto, é possível abrir um navegador e efetuar check-out do app. Para formar a URL, tome o mesmo endereço IP público que você usou na lição anterior para seu nó do trabalhador e combine-o com o NodePort que foi especificado no script de configuração. Para obter o endereço IP público para o nó do trabalhador:

  ```
  ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

  Com os valores de exemplo, a URL é `http://169.xx.xxx.xxx:30072`. Em um navegador, você poderá ver o texto a seguir. Se não vir este texto, não se preocupe. Este app está projetado para ficar ativo e inativo.

  ```
  Hello world! Bom trabalho em deixar o segundo estágio funcionando.
  ```
  {: screen}

  Também é possível verificar `http://169.xx.xxx.xxx:30072/healthz` para obter o status.

  Para os primeiros 10 - 15 segundos, uma mensagem 200 é retornada, assim você sabe que o app está sendo executado com êxito. Após esses 15 segundos, uma mensagem de tempo limite é exibida. Este é um comportamento esperado.

  ```
  {
    "error": "Timeout, Health check error!"
  }
  ```
  {: screen}

7.  Verifique seu status do pod para monitorar o funcionamento de seu app no Kubernetes. É possível verificar o status na CLI ou no painel do Kubernetes.

    *  **Na CLI**: consulte o que está acontecendo com seus pods à medida que eles mudam o status.
       ```
       kubectl get pods -o wide -w
       ```
       {: pre}

    *  **No painel do Kubernetes**:

       1.  [ Ativar o painel do Kubernetes ](/docs/containers?topic=containers-app#cli_dashboard).
       2.  Na guia **Cargas de trabalho**, é possível ver os recursos que você criou. Nessa guia, é possível atualizar continuamente e ver que a verificação de funcionamento está funcionando. Na seção **Pods**, é possível ver quantas vezes os pods são reiniciados quando os contêineres neles são recriados. Se acontecer de você capturar o erro a seguir no painel, esta mensagem indicará que a verificação de funcionamento capturou um problema. Aguarde alguns minutos e atualize novamente. Você verá o número de mudança de reinicializações para cada pod.

       ```
       Liveness probe failed: HTTP probe failed with statuscode: 500
    Back-off restarting failed docker container
    Error syncing pod, skipping: failed to "StartContainer" for "hw-container" with CrashLoopBackOff: "Back-off 1m20s restarting failed container=hw-container pod=hw-demo-deployment-3090568676-3s8v1_default(458320e7-059b-11e7-8941-56171be20503)"
       ```
       {: screen}

Bom trabalho! A segunda versão do app foi implementada. Você teve que usar menos comandos,
aprendeu como as verificações de funcionamento trabalham e editou uma implementação, o que é ótimo! O app Hello World passou no teste para a firma PR. Agora, é possível implementar um app mais útil para que a firma PR comece a analisar os press releases.

Pronto para excluir o que você criou antes de continuar? Desta vez, é possível usar o mesmo script de configuração para excluir ambos os recursos que você criou.

  ```
  kubectl delete -f healthcheck.yml
  ```
  {: pre}

  Saída de exemplo:

  ```
  deployment "hw-demo-deployment" deleted
service "hw-demo-service" deleted
  ```
  {: screen}

<br />


## Lição 3: implementando e atualizando o app Watson Tone Analyzer
{: #cs_apps_tutorial_lesson3}

Nas lições anteriores, os apps foram implementados como componentes únicos em um nó do trabalhador. Nesta lição, é possível implementar dois componentes de um app em um cluster que usam o serviço {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}.
{:shortdesc}

Separar os componentes em diferentes contêineres assegura que seja possível atualizar um sem afetar os outros. Em seguida, você atualiza o app para escalá-lo para cima com mais réplicas para torná-lo mais altamente disponível. O diagrama a seguir inclui os componentes que você implementa concluindo esta lição.

![Configuração de implementação](images/cs_app_tutorial_mz-components3.png)

No tutorial anterior, você tem a sua conta e um cluster com um nó do trabalhador. Nesta lição, você cria uma instância do serviço {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} em sua conta do {{site.data.keyword.Bluemix_notm}} e configura duas implementações, uma implementação para cada componente do app. Cada componente é implementado em um pod do Kubernetes no nó do trabalhador. Para tornar ambos os componentes publicamente disponíveis, você também cria um serviço do Kubernetes para cada componente.


### Lição 3a: implementando o app {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}
{: #lesson3a}

1.  Em uma CLI, navegue para o diretório `Lab 3`.

  ```
  cd 'container-service-getting-started-wt/Lab 3'
  ```
  {: pre}

2.  Se tiver iniciado uma nova sessão da CLI, efetue login e configure o contexto do cluster.

3.  Construa a primeira imagem {{site.data.keyword.watson}}.

    1.  Navegue para o diretório `watson`.

        ```
        cd watson
        ```
        {: pre}

    2.  Construa, identifique e envie por push o app `watson` como uma imagem para seu namespace no {{site.data.keyword.registryshort_notm}}. Novamente, não esqueça o ponto (`.`) no final do comando.

        ```
        ibmcloud cr build -t <region>.icr.io/<namespace>/watson .
        ```
        {: pre}

        Verifique se você vê a mensagem de êxito.

        ```
        Successfully built <image_id>
        ```
        {: screen}

4.  Construa a imagem de conversa do {{site.data.keyword.watson}}.

    1.  Navegue para o diretório `watson-talk`.

        ```
        cd 'container-service-getting-started-wt/Lab 3/watson-talk'
        ```
        {: pre}

    2.  Construa, identifique e envie por push o app `watson-talk` como uma imagem para seu namespace no {{site.data.keyword.registryshort_notm}}. Novamente, não esqueça o ponto (`.`) no final do comando.

        ```
        ibmcloud cr build -t <region>.icr.io/<namespace>/watson-talk .
        ```
        {: pre}

        Verifique se você vê a mensagem de êxito.

        ```
        Successfully built <image_id>
        ```
        {: screen}

5.  Verifique se as imagens foram incluídas com êxito em seu namespace de registro.

    ```
    imagens ibmcloud cr
    ```
    {: pre}

    Saída de exemplo:

    ```
    Listing images...

    REPOSITORY                        NAMESPACE  TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS
    us.icr.io/namespace/hello-world   namespace  1        0d90cb732881   40 minutes ago  264 MB   OK
    us.icr.io/namespace/hello-world   namespace  2        c3b506bdf33e   20 minutes ago  264 MB   OK
    us.icr.io/namespace/watson        namespace  latest   fedbe587e174   3 minutes ago   274 MB   OK
    us.icr.io/namespace/watson-talk   namespace  latest   fedbe587e174   2 minutes ago   274 MB   OK
    ```
    {: screen}

6.  Abra o arquivo `watson-deployment.yml` no diretório `Lab 3` com um editor de texto. Esse script de configuração inclui uma implementação e um serviço para ambos os componentes do app, `watson` e `watson-talk`.

    1.  Atualize os detalhes para a imagem em seu namespace de registro para ambas as implementações.

        watson:

        ```
        image: "<region>.icr.io/namespace/watson"
        ```
        {: codeblock}

        watson-talk:

        ```
        image: "<region>.icr.io/namespace/watson-talk"
        ```
        {: codeblock}

    2.  Na seção de volumes da implementação `watson-pod` atualize o nome do segredo do {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} criado no tutorial anterior [Criando o cluster Kubernetes](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial_lesson4). Montando o segredo do Kubernetes como um volume para a sua implementação, você torna a chave de API do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) disponível para o contêiner que está em execução em seu pod. Os componentes do app do {{site.data.keyword.watson}} neste tutorial são configurados para consultar a chave de API usando o caminho de montagem do volume.

        ```
        volumes:
                - name: service-bind-volume
                  secret:
                    defaultMode: 420
                    secretName: binding-mytoneanalyzer
        ```
        {: codeblock}

        Se você esqueceu como nomeou o segredo, execute o comando a seguir.

        ```
        kubectl get secrets --namespace=default
        ```
        {: pre}

    3.  Na seção do serviço watson-talk, anote o valor que está configurado para o `NodePort`. Esse exemplo usa 30080.

7.  Execute o script de configuração.

  ```
  kubectl apply -f watson-deployment.yml
  ```
  {: pre}

8.  Opcional: verifique se o segredo do {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} está montado como um volume para o pod.

    1.  Para obter o nome de um pod do watson, execute o comando a seguir.

        ```
        kubectl get pods
        ```
        {: pre}

        Saída de exemplo:

        ```
        NAME                                 READY     STATUS    RESTARTS  AGE
        watson-pod-4255222204-rdl2f          1/1       Running   0         13h
        watson-talk-pod-956939399-zlx5t      1/1       Running   0         13h
        ```
        {: screen}

    2.  Obtenha os detalhes sobre o pod e procure o nome do segredo.

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

        Saída de exemplo:

        ```
        Volumes:
          service-bind-volume:
            Type:       Secret (a volume populated by a Secret)
            SecretName: binding-mytoneanalyzer
          default-token-j9mgd:
            Type:       Secret (a volume populated by a Secret)
            SecretName: default-token-j9mgd
        ```
        {: codeblock}

9.  Abra um navegador e analise algum texto. O formato da URL é `http://<worker_node_IP_address>:<watson-talk-nodeport>/analyze/"<text_to_analyze>"`.

    Exemplo:

    ```
    http://169.xx.xxx.xxx:30080/analyze/"Today is a beautiful day"
    ```
    {: screen}

    Em um navegador, é possível ver a resposta JSON para o texto inserido.

10. [ Ativar o painel do Kubernetes ](/docs/containers?topic=containers-app#cli_dashboard).

11. Na guia **Cargas de trabalho**, é possível ver os recursos que você criou.

### Lição 3b. Atualizando a implementação em execução do Watson Tone Analyzer
{: #lesson3b}

Enquanto uma implementação está em execução, é possível editá-la para mudar valores no modelo de pod. Esta lição inclui atualizar a imagem que é usada. A firma PR deseja mudar o app na implementação.
{: shortdesc}

Mude o nome da imagem:

1.  Abra os detalhes da configuração para a implementação em execução.

    ```
    kubectl edit deployment/watson-talk-pod
    ```
    {: pre}

    Dependendo do seu sistema operacional, um editor de vi ou um editor de texto se abre.

2.  Mude o nome da imagem para a imagem ibmliberty.

    ```
    spec:
          containers:
          - image: <region>.icr.io/ibmliberty:latest
    ```
    {: codeblock}

3.  Salve as mudanças e saia do editor.

4.  Aplique as mudanças na implementação em execução.

    ```
    kubectl rollout status deployment/watson-talk-pod
    ```
    {: pre}

    Aguarde a confirmação de que o lançamento esteja completo.

    ```
    deployment "watson-talk-pod" successfully rolled out
    ```
    {: screen}

    Quando você apresenta uma mudança, outro pod é criado e testado pelo Kubernetes. Quando o teste é bem-sucedido, o pod antigo é removido.

[Teste seus conhecimentos e faça um teste! ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibmcloud-quizzes.mybluemix.net/containers/apps_tutorial/quiz.php)

Bom trabalho! Você implementou o app {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}. A firma PR pode começar a usar essa implementação para iniciar a análise de seus press releases.

Pronto para excluir o que você criou? É possível usar o script de configuração para excluir os recursos criados.

  ```
  kubectl delete -f watson-deployment.yml
  ```
  {: pre}

  Saída de exemplo:

  ```
  deployment "watson-pod" deleted
deployment "watson-talk-pod" deleted
service "watson-service" deleted
service "watson-talk-service" deleted
  ```
  {: screen}

  Se você não deseja manter o cluster, é possível excluí-lo também.

  ```
  ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
  ```
  {: pre}

## O que Vem a Seguir?
{: #apps_next}

Agora que você conquistou o básico, é possível mover para atividades mais avançadas. Considere experimentar um dos seguintes:

- Conclua um [laboratório mais complicado ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM/container-service-getting-started-wt#lab-overview) no repositório
- [Escalar automaticamente seus apps](/docs/containers?topic=containers-app#app_scaling) com o {{site.data.keyword.containerlong_notm}}
- Explore os padrões de código de orquestração de contêiner no [IBM Developer ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/technologies/containers/)
