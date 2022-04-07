---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# Tutorial: criando clusters do Kubernetes
{: #cs_cluster_tutorial}

Com este tutorial, é possível implementar e gerenciar um cluster do Kubernetes no {{site.data.keyword.containerlong}}. Saiba como automatizar a implementação, a operação, o ajuste de escala e o monitoramento de apps conteinerizados em um cluster.
{:shortdesc}

Nesta série de tutoriais, é possível ver como uma firma fictícia de relações públicas usa os recursos do Kubernetes para implementar um app conteinerizado no {{site.data.keyword.Bluemix_notm}}. Usando o {{site.data.keyword.toneanalyzerfull}}, o escritório de RP analisa seus press releases e recebe feedback.


## Objetivos
{: #tutorials_objectives}

Neste primeiro tutorial, você atua como administrador de networking do escritório de RP. Você configura um cluster do Kubernetes customizado que é usado para implementar e testar uma versão do Hello World do app no {{site.data.keyword.containerlong_notm}}.
{:shortdesc}

-   Crie um cluster com 1 conjunto de trabalhadores que tenha 1 nó do trabalhador.
-   Instale as CLIs para executar [comandos do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubectl.docs.kubernetes.io/) e gerenciando imagens do Docker no {{site.data.keyword.registrylong_notm}}.
-   Crie um repositório de imagem privada no {{site.data.keyword.registrylong_notm}} para armazenar as suas imagens.
-   Inclua o serviço {{site.data.keyword.toneanalyzershort}} no cluster para que qualquer app no cluster possa usar esse serviço.


## Tempo Necessário
{: #tutorials_time}

40
minutos


## Público
{: #tutorials_audience}

Este tutorial é destinado a desenvolvedores de software e administradores de rede que estão criando um cluster do Kubernetes pela primeira vez.
{: shortdesc}

## Pré-requisitos
{: #tutorials_prereqs}

-  Verifique as etapas que você precisa executar para [preparar-se para criar um cluster](/docs/containers?topic=containers-clusters#cluster_prepare).
-  Assegure-se de que tenha as políticas de acesso a seguir:
    - A [função da plataforma **Administrador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.containerlong_notm}}
    - A [função da plataforma **Administrador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.registrylong_notm}}
    - A função de serviço [**Gravador** ou **Gerenciador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.containerlong_notm}}


## Lição 1: criando um cluster e configurando a CLI
{: #cs_cluster_tutorial_lesson1}

Crie seu cluster do Kubernetes no console do {{site.data.keyword.Bluemix_notm}} e instale as CLIs necessárias.
{: shortdesc}

**Para criar seu cluster**

Como pode levar alguns minutos para provisão, crie seu cluster antes de instalar as CLIs.

1.  [No console do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/catalog/cluster/create), crie um cluster grátis ou padrão com 1 conjunto de trabalhadores que contenha 1 nó do trabalhador.

    Também é possível criar um [cluster na CLI](/docs/containers?topic=containers-clusters#clusters_cli_steps).
    {: tip}

Como seu cluster é provisionado, instale as CLIs a seguir que são usadas para gerenciar clusters:
-   CLI do {{site.data.keyword.Bluemix_notm}}
-   Plug-in do {{site.data.keyword.containerlong_notm}}
-   Kubernetes CLI
-   Plug-in do {{site.data.keyword.registryshort_notm}}

Se, em vez disso, desejar usar o console do {{site.data.keyword.Bluemix_notm}} após a criação de seu cluster, será possível executar comandos da CLI diretamente de seu navegador da web no [Terminal do Kubernetes](/docs/containers?topic=containers-cs_cli_install#cli_web).
{: tip}

</br>
**Para instalar as CLIs e os seus pré-requisitos**

1. Instale o [{{site.data.keyword.Bluemix_notm}} CLI ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-getting-started). Esta instalação inclui:
  - A CLI de base do {{site.data.keyword.Bluemix_notm}}. O prefixo para executar comandos usando a CLI do {{site.data.keyword.Bluemix_notm}} é `ibmcloud`.
  - O plug-in do {{site.data.keyword.containerlong_notm}}. O prefixo para executar comandos usando a CLI do {{site.data.keyword.Bluemix_notm}} é `ibmcloud ks`.
  - Plug-in do {{site.data.keyword.registryshort_notm}}. Use esse plug-in para configurar e gerenciar um repositório de imagem privada no {{site.data.keyword.registryshort_notm}}. O prefixo para executar comandos de registro é `ibmcloud cr`.

2. Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas.
  ```
  ibmcloud login
  ```
  {: pre}

  Se você tiver um ID federado, use o sinalizador `--sso` para efetuar login. Insira seu nome do usuário e use a
URL fornecida na saída da CLI para recuperar sua senha descartável.
  {: tip}

3. Siga os prompts para selecionar uma conta.

5. Verifique se os plug-ins estão instalados corretamente.
  ```
  ibmcloud plugin list
  ```
  {: pre}

  O plug-in do {{site.data.keyword.containerlong_notm}} é exibido nos resultados como **container-service** e o plug-in do {{site.data.keyword.registryshort_notm}} é exibido nos resultados como **container-registry**.

6. Para implementar apps em seus clusters, [instale a CLI do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). O prefixo para executar comandos usando o Kubernetes CLI é `kubectl`.

  1. Faça download da versão `major.minor` da CLI do Kubernetes que corresponda à versão `major.minor` do cluster do Kubernetes que você planeja usar. A versão atual do Kubernetes padrão do {{site.data.keyword.containerlong_notm}} é 1.13.6.
    - **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    - **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    - **Windows**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

  2. Se você estiver usando o OS X ou Linux, conclua as etapas a seguir.

    1. Mova o arquivo executável para o diretório `/usr/local/bin`.
      ```
      Mv /filepath/kubectl /usr/local/bin/kubectl
      ```
      {: pre}

    2. Certifique-se de que `/usr/local/bin` esteja listado na variável do sistema `PATH`. A variável `PATH` contém todos os diretórios nos quais o sistema operacional pode localizar arquivos executáveis. Os diretórios que estão listados na variável `PATH` servem propósitos diferentes. `/usr/local/bin` é usado para armazenar arquivos executáveis para o software que não faz parte do sistema operacional e que foi instalado manualmente pelo administrador do sistema.
      ```
      echo $PATH
      ```
      {: pre}
      Exemplo de saída da CLI:
      ```
      /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
      ```
      {: screen}

    3. Torne o arquivo executável.
      ```
      chmod +x /usr/local/bin/kubectl
      ```
      {: pre}

Bom trabalho! Você instalou com êxito as CLIs para as lições e os tutoriais a seguir. Em seguida, configure seu ambiente em cluster e inclua o serviço {{site.data.keyword.toneanalyzershort}}.


## Lição 2: configurando seu registro privado
{: #cs_cluster_tutorial_lesson2}

Configure um repositório de imagem privada no {{site.data.keyword.registryshort_notm}} e inclua segredos em seu cluster do Kubernetes para que o app possa acessar o serviço {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

1.  Se seu cluster estiver em um grupo de recursos diferente do `default`, torne esse grupo de recursos o destino. Para ver o grupo de recursos ao qual cada cluster pertence, execute `ibmcloud ks clusters`.
   ```
   ibmcloud target -g <resource_group_name>
   ```
   {: pre}

2.  Configure o seu próprio repositório de imagem privada no {{site.data.keyword.registryshort_notm}} para armazenar com segurança e compartilhar imagens do Docker com todos os usuários do cluster. Um repositório de imagem privada no {{site.data.keyword.Bluemix_notm}} é identificado por um namespace. O namespace é usado para criar uma URL exclusiva para o seu repositório de imagem que
os desenvolvedores podem usar para acessar imagens privadas do Docker.

    Saiba mais sobre [como proteger suas informações pessoais](/docs/containers?topic=containers-security#pi) quando trabalhar com imagens de contêiner.

    Neste exemplo, a firma PR deseja criar somente um repositório de imagem no {{site.data.keyword.registryshort_notm}}; portanto, eles escolhem `pr_firm` como o seu namespace para agrupar todas as imagens em sua conta. Substitua &lt;namespace&gt; por um namespace de sua preferência que não esteja relacionado ao tutorial.

    ```
    ibmcloud cr namespace-add <namespace>
    ```
    {: pre}

3.  Antes de continuar com a próxima etapa, verifique se a implementação de seu nó do trabalhador está concluída.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Quando seu nó do trabalhador conclui o fornecimento, o status muda para **Pronto** e é possível iniciar a ligação de serviços do {{site.data.keyword.Bluemix_notm}}.

    ```
    ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
    kube-mil01-pafe24f557f070463caf9e31ecf2d96625-w1   169.xx.xxx.xxx   10.xxx.xx.xxx   free           normal   Ready    mil01      1.13.6
    ```
    {: screen}

## Lição 3: configurando seu ambiente em cluster
{: #cs_cluster_tutorial_lesson3}

Configure o contexto para seu cluster do Kubernetes em sua CLI.
{: shortdesc}

Toda vez que você efetua login na CLI do {{site.data.keyword.containerlong}} para trabalhar com clusters, deve-se executar esses comandos para configurar o caminho para o arquivo de configuração do cluster como uma variável de sessão. O Kubernetes CLI usa essa variável para localizar um arquivo de configuração local e certificados que são necessárias para se conectar ao cluster no {{site.data.keyword.Bluemix_notm}}.

1.  Obtenha o comando para configurar a variável de ambiente e fazer download dos arquivos de configuração
do Kubernetes.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Quando o download dos arquivos de configuração estiver concluído, será exibido um comando que poderá ser usado para configurar o caminho para o seu arquivo de configuração local do Kubernetes como uma variável de ambiente.

    Exemplo para OS X:
    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}
    Usando o Windows PowerShell? Inclua a sinalização `--powershell` para obter variáveis de ambiente no formato PowerShell do Windows.
    {: tip}

2.  Copie e cole o comando que é exibido em seu terminal para configurar a variável de ambiente `KUBECONFIG`.

3.  Verifique se a variável de ambiente `KUBECONFIG` está configurada corretamente.

    Exemplo para OS X:

    ```
    echo $KUBECONFIG
    ```
    {: pre}

    Saída:

    ```
    /Users/<user_name>/.bluemix/plugins/container-service/clusters/pr_firm_cluster/kube-config-prod-par02-pr_firm_cluster.yml
    ```
    {: screen}

4.  Verifique se os comandos `kubectl` são executados adequadamente com seu cluster verificando a versão do servidor de CLI do Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Saída de exemplo:

    ```
    Versão do cliente: v1.13.6 Versão do servidor: v1.13.6
    ```
    {: screen}

## Lição 4: incluindo um serviço em seu cluster
{: #cs_cluster_tutorial_lesson4}

Com serviços {{site.data.keyword.Bluemix_notm}}, é possível aproveitar a funcionalidade já desenvolvida nos apps. Qualquer serviço do {{site.data.keyword.Bluemix_notm}} que está ligado ao cluster do Kubernetes pode ser usado por qualquer app que esteja implementado nesse cluster. Repita as etapas a seguir para cada serviço do {{site.data.keyword.Bluemix_notm}} que você deseja usar com seus apps.
{: shortdesc}

1.  Inclua o serviço {{site.data.keyword.toneanalyzershort}} em sua conta do {{site.data.keyword.Bluemix_notm}}. Substitua <service_name> por um nome para sua instância de serviço.

    Quando você inclui o serviço do {{site.data.keyword.toneanalyzershort}} em sua conta, uma mensagem é exibida de que o serviço não está livre. Se você limitar sua chamada API, este tutorial não incorrerá em encargos do serviço {{site.data.keyword.watson}}. [Revise as informações de precificação para o serviço {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog/services/tone-analyzer).
    {: note}

    ```
    ibmcloud resource service-instance-create <service_name> tone-analyzer standard us-south
    ```
    {: pre}

2.  Ligue a instância do {{site.data.keyword.toneanalyzershort}} ao namespace `padrão` do Kubernetes para o cluster. Posteriormente, será possível criar seus próprios namespaces para gerenciar o acesso de usuário aos recursos do Kubernetes, mas por enquanto, use o namespace `padrão`. Os namespaces do Kubernetes são diferentes do namespace de registro que você criou anteriormente.

    ```
    ibmcloud ks cluster-service-bind --cluster <cluster_name> --namespace default --service <service_name>
    ```
    {: pre}

    Saída:

    ```
    ibmcloud ks cluster-service-bind pr_firm_cluster default mytoneanalyzer
    Binding service instance to namespace...
    OK
    Namespace:	default
    Secret name:	binding-mytoneanalyzer
    ```
    {: screen}

3.  Verifique se o segredo do Kubernetes foi criado em seu namespace do cluster. Cada serviço do {{site.data.keyword.Bluemix_notm}} é definido por um arquivo JSON que inclui informações confidenciais, como a chave de API do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) e a URL que o contêiner usa para obter acesso. Para armazenar essas informações com segurança, segredos do Kubernetes são usados. Neste exemplo, o segredo inclui a chave de API para acessar a instância do {{site.data.keyword.watson}}{{site.data.keyword.toneanalyzershort}} que é provisionado em sua conta.

    ```
    kubectl get secrets --namespace=default
    ```
    {: pre}

    Saída:

    ```
    NAME                       TYPE                                  DATA      AGE
    binding-mytoneanalyzer     Opaque                                1         1m
    bluemix-default-secret     kubernetes.io/dockercfg               1         1h
    default-token-kf97z        kubernetes.io/service-account-token   3         1h
    ```
    {: screen}

</br>
Bom trabalho! Seu cluster está configurado e seu ambiente local está pronto para você começar a implementar apps no cluster.

## O que Vem a Seguir?
{: #tutorials_next}

* Teste seu conhecimento e [faça este questionário ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibmcloud-quizzes.mybluemix.net/containers/cluster_tutorial/quiz.php)!
* Tente o [Tutorial: implementando apps em clusters do Kubernetes](/docs/containers?topic=containers-cs_apps_tutorial#cs_apps_tutorial) para implementar o app da firma PR no cluster que você criou.
