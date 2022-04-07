---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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



# Configurando a CLI e API
{: #cs_cli_install}

É possível usar a CLI ou API do {{site.data.keyword.containerlong}} para criar e gerenciar seus clusters do Kubernetes.
{:shortdesc}

## Instalando a CLI e os plug-ins do IBM Cloud
{: #cs_cli_install_steps}

Instale as CLIs necessárias para criar e gerenciar seus clusters do Kubernetes no {{site.data.keyword.containerlong_notm}} e implementar apps conteinerizados em seu cluster.
{:shortdesc}

Esta tarefa inclui as informações para instalar essas CLIs e plug-ins:

-   CLI do {{site.data.keyword.Bluemix_notm}}
-   Plug-in do {{site.data.keyword.containerlong_notm}}
-   Plug-in do {{site.data.keyword.registryshort_notm}}

Se, em vez disso, desejar usar o console do {{site.data.keyword.Bluemix_notm}} após a criação de seu cluster, será possível executar comandos da CLI diretamente de seu navegador da web no [Terminal do Kubernetes](#cli_web).
{: tip}

<br>
Para instalar as CLIs:

1.  Instale o [{{site.data.keyword.Bluemix_notm}} CLI ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-getting-started#idt-prereq). Esta instalação inclui:
    -   A CLI do {{site.data.keyword.Bluemix_notm}} base (`ibmcloud`).
    -   O plug-in {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`).
    -   Plug-in {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`). Use esse plug-in para configurar o seu próprio namespace em um registro de imagem privada de múltiplos locatários, altamente disponível e escalável que é hospedado pela IBM e para armazenar e compartilhar imagens do Docker com outros usuários. As imagens do Docker são necessárias para implementar contêineres em um cluster.
    -   A CLI do Kubernetes (`kubectl`) que corresponde à versão padrão: 1.13.6.<p class="note">Se você planeja usar um cluster que executa uma versão diferente, talvez seja necessário [instalar essa versão da CLI do Kubernetes separadamente](#kubectl). Se tiver um cluster (OpenShift), você [instalará as CLIs `oc` e `kubectl` juntas](#cli_oc).</p>
    -   A CLI do Helm (`helm`). Você pode usar o Helm como um gerenciador de pacote para instalar serviços do {{site.data.keyword.Bluemix_notm}} e apps complexos em seu cluster por meio de gráficos Helm. Deve-se ainda [configurar o Helm](/docs/containers?topic=containers-helm) em cada cluster no qual deseja usar o Helm.

    Planeja usar muito a CLI? Tente [Ativando a conclusão automática de shell para a CLI do {{site.data.keyword.Bluemix_notm}} (somente Linux/MacOS)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas.
    ```
    ibmcloud login
    ```
    {: pre}

    Se você tiver um ID federado, use `ibmcloud login --sso` para efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável. Você sabe que tem um ID federado quando o login falha sem a opção `--sso` e é bem-sucedido com a opção `--sso`.
    {: tip}

3.  Verifique se o plug-in {{site.data.keyword.containerlong_notm}} e os plug-ins {{site.data.keyword.registryshort_notm}} estão instalados corretamente.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    Saída de exemplo:
    ```
    Listando plug-ins instalados...

    Plugin Name                            Version   Status        
    container-registry                     0.1.373     
    container-service/kubernetes-service   0.3.23   
    ```
    {: screen}

Para obter informações de referência sobre essas CLIs, veja a documentação para essas ferramentas.

-   [Comandos `ibmcloud`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [Comandos `ibmcloud ks`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)
-   [Comandos `ibmcloud cr`](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli)

## Instalando a CLI do Kubernetes (`kubectl`)
{: #kubectl}

Para visualizar uma versão local do painel do Kubernetes e para implementar apps em seus clusters, instale a CLI do Kubernetes (`kubectl`). A versão estável mais recente do `kubectl` é instalada com a CLI base do {{site.data.keyword.Bluemix_notm}}. No entanto, para trabalhar com seu cluster, deve-se, em vez disso, instalar a versão `major.minor` da CLI do Kubernetes, que corresponde à versão `major.minor` do cluster Kubernetes que você planeja usar. Se você usar uma versão da CLI `kubectl` que não corresponda pelo menos à versão `major.minor` de seus clusters, poderá ter resultados inesperados. Por exemplo, [o Kubernetes não suporta ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/setup/version-skew-policy/) versões do cliente `kubectl` que tem 2 ou mais versões de diferença da versão do servidor (n +/- 2). Certifique-se de manter as versões de cluster e de CLI do Kubernetes atualizadas.
{: shortdesc}

Usando um cluster do OpenShift? Instale a CLI do OpenShift Origin (`oc`) no lugar, que é fornecido com `kubectl`. Se você tiver os clusters nativos do {{site.data.keyword.containershort_notm}} do Red Hat OpenShift on IBM Cloud e do Ubuntu, certifique-se de usar o arquivo binário `kubectl` que corresponde à versão do Kubernetes `major.minor` do cluster.
{: tip}

1.  Se você já tiver um cluster, verifique se a versão da CLI do `kubectl` do cliente corresponde à versão do servidor de API do cluster.
    1.  [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  Compare as versões do cliente e do servidor. Se o cliente não corresponder ao servidor, continue com a próxima etapa. Se as versões corresponderem, você já instalou a versão apropriada de `kubectl`.
        ```
        kubectl version  --short
        ```
        {: pre}
2.  Faça download da versão `major.minor` da CLI do Kubernetes que corresponda à versão `major.minor` do cluster do Kubernetes que você planeja usar. A versão atual do Kubernetes padrão do {{site.data.keyword.containerlong_notm}} é 1.13.6.
    -   **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/darwin/amd64/kubectl)
    -   **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/linux/amd64/kubectl)
    -   **Windows**: instale a CLI do Kubernetes no mesmo diretório da CLI do {{site.data.keyword.Bluemix_notm}}. Essa configuração economiza algumas mudanças de caminho de arquivo ao executar comandos posteriormente. [https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://storage.googleapis.com/kubernetes-release/release/v1.13.6/bin/windows/amd64/kubectl.exe)

3.  Se você usar OS X ou Linux, conclua as etapas a seguir.
    1.  Mova o arquivo executável para o diretório `/usr/local/bin`.
        ```
        mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Certifique-se de que `/usr/local/bin` esteja listado na variável do sistema `PATH`. A variável `PATH` contém todos os diretórios nos quais o sistema operacional pode localizar arquivos executáveis. Os diretórios que estão listados na variável `PATH` servem propósitos diferentes. `/usr/local/bin` é usado para armazenar arquivos executáveis para o software que não faz parte do sistema operacional e que foi instalado manualmente pelo administrador do sistema.
        ```
        echo $PATH
        ```
        {: pre}
        Exemplo de saída da CLI:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}

    3.  Torne o arquivo executável.
        ```
        chmod +x /usr/local/bin/kubectl
        ```
        {: pre}
4.  **Opcional**: [ative a conclusão automática para comandos `kubectl` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). As etapas variam, dependendo do shell usado.

Em seguida, inicie [Criando clusters do Kubernetes por meio da CLI com o {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters#clusters_cli_steps).

Para obter mais informações sobre a CLI do Kubernetes, consulte os [docs de referência do `kubectl` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubectl.docs.kubernetes.io/).
{: note}

<br />


## Instalando o beta de visualização da CLI do OpenShift Origin (`oc`)
{: #cli_oc}

O [Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) está disponível como um beta para testar clusters do OpenShift.
{: preview}

Para visualizar uma versão local do painel do OpenShift e para implementar apps em seus clusters do Red Hat OpenShift on IBM Cloud, instale a CLI do OpenShift Origin (`oc`). A CLI `oc` inclui uma versão correspondente da CLI do Kubernetes (`kubectl`). Para obter mais informações, consulte os [docs do OpenShift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html).
{: shortdesc}

Usando clusters nativos do {{site.data.keyword.containershort_notm}} do Red Hat OpenShift on IBM Cloud e do Ubuntu? A CLI `oc` é fornecida com os binários `oc` e `kubectl`, mas seus clusters diferentes podem executar versões diferentes do Kubernetes, como 1.11 no OpenShift e 1.13.6 no Ubuntu. Certifique-se de usar o binário `kubectl` que corresponde à versão do Kubernetes `major.minor` do cluster.
{: note}

1.  [Faça download da CLI do OpenShift Origin ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.okd.io/download.html) para seu sistema operacional local e versão do OpenShift. A versão padrão do OpenShift atual é 3.11.

2.  Se você usar Mac OS ou Linux, conclua as etapas a seguir para incluir os binários em sua variável do sistema `PATH`. Se você usar o Windows, instale a CLI `oc` no mesmo diretório que a CLI do {{site.data.keyword.Bluemix_notm}}. Essa configuração economiza algumas mudanças de caminho de arquivo ao executar comandos posteriormente.
    1.  Mova os arquivos executáveis `oc` e `kubectl` para o diretório `/usr/local/bin`.
        ```
        mv /<filepath>/oc /usr/local/bin/oc && mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Certifique-se de que `/usr/local/bin` esteja listado na variável do sistema `PATH`. A variável `PATH` contém todos os diretórios nos quais o sistema operacional pode localizar arquivos executáveis. Os diretórios que estão listados na variável `PATH` servem propósitos diferentes. `/usr/local/bin` é usado para armazenar arquivos executáveis para o software que não faz parte do sistema operacional e que foi instalado manualmente pelo administrador do sistema.
        ```
        echo $PATH
        ```
        {: pre}
        Exemplo de saída da CLI:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}
3.  **Opcional**: [ative a conclusão automática para comandos `kubectl` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). As etapas variam, dependendo do shell usado. É possível repetir as etapas para ativar a conclusão automática para comandos `oc`. Por exemplo, em bash no Linux, em vez de `kubectl completion bash >/etc/bash_completion.d/kubectl`, é possível executar `oc completion bash >/etc/bash_completion.d/oc_completion`.

Em seguida, inicie [Criando um cluster do Red Hat OpenShift on IBM Cloud (visualização)](/docs/containers?topic=containers-openshift_tutorial).

Para obter mais informações sobre a CLI do OpenShift Origin, consulte os [docs de comandos `oc` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/cli_reference/basic_cli_operations.html).
{: note}

<br />


## Executando a CLI em um contêiner em seu computador
{: #cs_cli_container}

Em vez de instalar cada uma das CLIs individualmente em seu computador, é possível instalar as CLIs em um contêiner que é executado em seu computador.
{:shortdesc}

Antes de iniciar, [instale o Docker ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.docker.com/community-edition#/download) para construir e executar imagens localmente. Se você estiver usando o Windows 8 ou anterior, será possível instalar o [Docker Toolbox ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) como alternativa.

1. Crie uma imagem do Dockerfile fornecido.

    ```
    Docker build -t < image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Implemente a imagem localmente como um contêiner e monte um volume para acessar arquivos locais.

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. Inicie executando os comandos `ibmcloud ks` e `kubectl` no shell interativo. Se você criar dados que deseja salvar, salve esses dados no volume montado. Quando você sair do shell, o contêiner será parado.

<br />



## Configurando a CLI para executar `kubectl`
{: #cs_cli_configure}

É possível usar os comandos que são fornecidos com a CLI do Kubernetes para gerenciar clusters no {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Todos os comandos `kubectl` que estão disponíveis no Kubernetes 1.13.6 são suportados para uso com clusters no {{site.data.keyword.Bluemix_notm}}. Após criar um cluster, configure o contexto para sua CLI local para esse cluster com uma variável de ambiente. Então, é possível executar os comandos `kubectl` do Kubernetes para trabalhar com o seu cluster no {{site.data.keyword.Bluemix_notm}}.

Antes de poder executar os comandos `kubectl`:
* [ Instale as CLIs necessárias ](#cs_cli_install).
* [Criar um cluster](/docs/containers?topic=containers-clusters#clusters_cli_steps).
* Certifique-se de que você tenha uma [função de serviço](/docs/containers?topic=containers-users#platform) que conceda a função RBAC apropriada do Kubernetes, para que seja possível trabalhar com recursos do Kubernetes. Se você tiver apenas uma função de serviço, mas nenhuma função da plataforma, precisará do administrador de cluster para fornecer a você o nome do cluster e o ID ou a função da plataforma **Visualizador** para listar clusters.

Para usar os comandos `kubectl`:

1.  Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas.

    ```
    ibmcloud login
    ```
    {: pre}

    Se você tiver um ID federado, use `ibmcloud login --sso` para efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável. Você sabe que tem um ID federado quando o login falha sem a opção `--sso` e é bem-sucedido com a opção `--sso`.
    {: tip}

2.  Selecione uma conta do {{site.data.keyword.Bluemix_notm}}. Se você estiver designado para múltiplas organizações do {{site.data.keyword.Bluemix_notm}}, selecione a organização na qual o cluster foi criado. Os clusters são específicos para uma organização, mas são independentes de um espaço do {{site.data.keyword.Bluemix_notm}}. Portanto, não é necessário selecionar um espaço.

3.  Para criar e trabalhar com clusters em um grupo de recursos diferente do padrão, destine esse grupo de recursos. Para ver o grupo de recursos ao qual cada cluster pertence, execute `ibmcloud ks clusters`. **Nota**: deve-se ter [acesso de **Visualizador**](/docs/containers?topic=containers-users#platform) ao grupo de recursos.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  Liste todos os clusters na conta para obter o nome do cluster. Se você tiver apenas uma função de serviço do {{site.data.keyword.Bluemix_notm}} IAM e não puder visualizar clusters, solicite ao administrador de cluster a função **Visualizador** da plataforma IAM ou o nome e ID do cluster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

5.  Configure o cluster criado como o contexto para esta sessão. Conclua estas etapas de configuração toda vez que você trabalhar com o seu cluster.
    1.  Obtenha o comando para configurar a variável de ambiente e fazer download dos arquivos de configuração
do Kubernetes. <p class="tip">Usando o Windows PowerShell? Inclua a sinalização `--powershell` para obter variáveis de ambiente no formato PowerShell do Windows.</p>
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Depois de fazer download dos arquivos de configuração, será exibido um comando que poderá ser usado para configurar o caminho para o arquivo de configuração local do Kubernetes como uma variável de ambiente.

        Exemplo:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copie e cole o comando que é exibido em seu terminal para configurar a variável de ambiente `KUBECONFIG`.

        **Usuários do Mac ou do Linux**: em vez de executar o comando `ibmcloud ks cluster-config` e copiar a variável de ambiente `KUBECONFIG`, é possível executar `ibmcloud ks cluster-config --export <cluster-name>`. Dependendo de seu shell, é possível configurá-lo executando `eval $(ibmcloud ks cluster-config --export <cluster-name>)`.
        {: tip}

    3.  Verifique se a variável de ambiente `KUBECONFIG` está configurada corretamente.

        Exemplo:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Saída:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Verifique se os comandos `kubectl` são executados adequadamente com seu cluster verificando a versão do servidor de CLI do Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Saída de exemplo:

    ```
    Versão do cliente: v1.13.6 Versão do servidor: v1.13.6
    ```
    {: screen}

Agora, é possível executar comandos `kubectl` para gerenciar seus clusters no {{site.data.keyword.Bluemix_notm}}. Para obter uma lista completa de comandos, veja a [documentação do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubectl.docs.kubernetes.io/).

**Dica:** se estiver usando o Windows e a CLI do Kubernetes não estiver instalada no mesmo diretório que a CLI do {{site.data.keyword.Bluemix_notm}}, você deverá mudar os diretórios para o caminho no qual a CLI do Kubernetes está instalada para executar comandos `kubectl` com êxito.


<br />




## Atualizando a CLI
{: #cs_cli_upgrade}

Talvez você queira atualizar as CLIs periodicamente para usar novos recursos.
{:shortdesc}

Esta tarefa inclui as informações para atualizar essas CLIs.

-   {{site.data.keyword.Bluemix_notm}}  CLI versão 0.8.0 ou mais recente
-   Plug-in do {{site.data.keyword.containerlong_notm}}
-   CLI do Kubernetes versão 1.13.6 ou mais recente
-   Plug-in do {{site.data.keyword.registryshort_notm}}

<br>
Para atualizar as CLIs:

1.  Atualize a CLI do {{site.data.keyword.Bluemix_notm}}. Faça download da [versão mais recente ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-getting-started) e execute o instalador.

2. Efetue login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira suas credenciais do {{site.data.keyword.Bluemix_notm}} quando solicitadas.

    ```
    ibmcloud login
    ```
    {: pre}

     Se você tiver um ID federado, use `ibmcloud login --sso` para efetuar login na CLI do {{site.data.keyword.Bluemix_notm}}. Insira seu nome do usuário e use a URL fornecida na saída da CLI para recuperar sua senha descartável. Você sabe que tem um ID federado quando o login falha sem a opção `--sso` e é bem-sucedido com a opção `--sso`.
     {: tip}

3.  Atualize o plug-in {{site.data.keyword.containerlong_notm}}.
    1.  Instale a atualização do repositório do plug-in do {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-service 
        ```
        {: pre}

    2.  Verifique a instalação do plug-in executando o comando a seguir e verificando a lista dos plug-ins instalados.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        O plug-in do {{site.data.keyword.containerlong_notm}} é exibido nos resultados como container-service.

    3.  Inicialize a CLI.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Atualize a CLI do Kubernetes](#kubectl).

5.  Atualize o plug-in {{site.data.keyword.registryshort_notm}}.
    1.  Instale a atualização do repositório do plug-in do {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  Verifique a instalação do plug-in executando o comando a seguir e verificando a lista dos plug-ins instalados.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        O plug-in de registro é exibido nos resultados como registro do contêiner.

<br />


## Desinstalando a CLI
{: #cs_cli_uninstall}

Se você não precisar mais da CLI, será possível desinstalá-la.
{:shortdesc}

Esta tarefa inclui as informações para remover estas CLIs:


-   Plug-in do {{site.data.keyword.containerlong_notm}}
-   Kubernetes CLI
-   Plug-in do {{site.data.keyword.registryshort_notm}}

Para desinstalar as CLIs:

1.  Desinstale o plug-in do {{site.data.keyword.containerlong_notm}}.

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  Desinstale o plug-in do {{site.data.keyword.registryshort_notm}}.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  Verifique se os plug-ins foram desinstalados executando o comando a seguir e verificando a lista dos plug-ins instalados.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    Os plug-ins container-service e container-registry não são exibidos nos resultados.

<br />


## Usando o Terminal do Kubernetes em seu navegador da web (beta)
{: #cli_web}

O Terminal do Kubernetes permite que você use a CLI do {{site.data.keyword.Bluemix_notm}} para gerenciar seu cluster diretamente de seu navegador da web.
{: shortdesc}

O Terminal do Kubernetes é liberado como um complemento beta do {{site.data.keyword.containerlong_notm}} e pode mudar devido ao feedback do usuário e a testes adicionais. Não use esse recurso em clusters de produção para evitar efeitos colaterais inesperados.
{: important}

Se você usar o painel do cluster no console do {{site.data.keyword.Bluemix_notm}} para gerenciar os seus clusters, mas desejar fazer mudanças de configuração mais avançadas rapidamente, agora será possível executar comandos da CLI diretamente de seu navegador da web no Terminal do Kubernetes. O Terminal do Kubernetes é ativado com a [CLI do {{site.data.keyword.Bluemix_notm}} base ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli?topic=cloud-cli-getting-started), o plug-in {{site.data.keyword.containerlong_notm}} e o plug-in {{site.data.keyword.registryshort_notm}}. Além disso, o contexto do terminal já está configurado para o cluster com o qual você está trabalhando para que seja possível executar comandos `kubectl` do Kubernetes para trabalhar com seu cluster.

Todos os arquivos transferidos por download e editados localmente, como arquivos YAML, são armazenados temporariamente no Terminal do Kubernetes e não persistem nas sessões.
{: note}

Para instalar e iniciar o Terminal do Kubernetes:

1.  Efetue login no  [ console do {{site.data.keyword.Bluemix_notm}}  ](https://cloud.ibm.com/).
2.  Na barra de menus, selecione a conta que você deseja usar.
3.  No menu ![Ícone de menu](../icons/icon_hamburger.svg "Ícone de menu"), clique em **Kubernetes**.
4.  Na página **Clusters**, clique no cluster que você deseja acessar.
5.  Na página de detalhes do cluster, clique no botão **Terminal**.
6.  Clique em **Instalar**. Pode levar alguns minutos para o complemento do terminal ser instalado.
7.  Clique no botão **Terminal** novamente. O terminal é aberto em seu navegador.

Na próxima vez, será possível iniciar o Terminal do Kubernetes simplesmente clicando no botão **Terminal**.

<br />


## Automatizando implementações de cluster com a API
{: #cs_api}

É possível usar a API do {{site.data.keyword.containerlong_notm}} para automatizar a criação, a implementação e o gerenciamento de seus clusters do Kubernetes.
{:shortdesc}

A API do {{site.data.keyword.containerlong_notm}} requer informações do cabeçalho que devem ser fornecidas na solicitação de API e que podem variar, dependendo da API que você deseja usar. Para determinar quais informações do cabeçalho são necessárias para sua API, consulte a [documentação da API do {{site.data.keyword.containerlong_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://us-south.containers.cloud.ibm.com/swagger-api).

Para se autenticar com o {{site.data.keyword.containerlong_notm}}, deve-se fornecer um token do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) que é gerado com suas credenciais do {{site.data.keyword.Bluemix_notm}} e que inclui o ID da conta do {{site.data.keyword.Bluemix_notm}} em que o cluster foi criado. Dependendo da maneira que você autenticar com o {{site.data.keyword.Bluemix_notm}}, será possível escolher entre as opções a seguir para automatizar a criação de seu token do {{site.data.keyword.Bluemix_notm}} IAM.

Também é possível usar o [arquivo JSON do swagger de API ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json) para gerar um cliente que possa interagir com a API como parte de seu trabalho de automação.
{: tip}

<table summary="Tipos e opções de ID com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
<caption>Tipos de ID e opções</caption>
<thead>
<th>ID do {{site.data.keyword.Bluemix_notm}}</th>
<th>Minhas opções</th>
</thead>
<tbody>
<tr>
<td>ID não federado</td>
<td><ul><li><strong>Gere uma chave de API do {{site.data.keyword.Bluemix_notm}}: </strong> como uma alternativa para usar o nome do usuário e a senha do {{site.data.keyword.Bluemix_notm}}, é possível <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">usar chaves de API do {{site.data.keyword.Bluemix_notm}}</a>. As chaves de API do {{site.data.keyword.Bluemix_notm}} são dependentes da conta do {{site.data.keyword.Bluemix_notm}} para a qual são geradas. Não é possível combinar sua chave de API do {{site.data.keyword.Bluemix_notm}} com um ID de conta diferente no mesmo token do IAM do {{site.data.keyword.Bluemix_notm}}. Para acessar os clusters que foram criados com uma conta diferente daquela na qual a sua chave API do {{site.data.keyword.Bluemix_notm}} é baseada; deve-se efetuar login na conta para gerar uma nova chave API.</li>
<li><strong>Nome de usuário e senha do {{site.data.keyword.Bluemix_notm}}:</strong> é possível seguir as etapas neste tópico para automatizar totalmente a criação de seu token de acesso do {{site.data.keyword.Bluemix_notm}} IAM.</li></ul>
</tr>
<tr>
<td>ID federado</td>
<td><ul><li><strong>Gerar uma chave de API do {{site.data.keyword.Bluemix_notm}}:</strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">chaves de API do {{site.data.keyword.Bluemix_notm}}</a> são dependentes da conta do {{site.data.keyword.Bluemix_notm}} para a qual elas são geradas. Não é possível combinar sua chave de API do {{site.data.keyword.Bluemix_notm}} com um ID de conta diferente no mesmo token do IAM do {{site.data.keyword.Bluemix_notm}}. Para acessar os clusters que foram criados com uma conta diferente daquela na qual a sua chave API do {{site.data.keyword.Bluemix_notm}} é baseada; deve-se efetuar login na conta para gerar uma nova chave API.</li>
<li><strong>Use uma senha descartável:</strong> se você autenticar com o {{site.data.keyword.Bluemix_notm}} usando uma senha descartável, não será possível automatizar totalmente a criação de seu token do {{site.data.keyword.Bluemix_notm}} IAM porque a recuperação de sua senha única requer uma interação manual com seu navegador da web. Para automatizar totalmente a criação de seu token do {{site.data.keyword.Bluemix_notm}} IAM, deve-se criar uma chave de API do {{site.data.keyword.Bluemix_notm}}.</ul></td>
</tr>
</tbody>
</table>

1.  Crie seu token de acesso do {{site.data.keyword.Bluemix_notm}} IAM. As informações do corpo incluídas em sua solicitação variam com base no método de autenticação do {{site.data.keyword.Bluemix_notm}} usado.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parâmetros de entrada para recuperar tokens do IAM com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
    <caption>Parâmetros de entrada para obter tokens do IAM.</caption>
    <thead>
        <th>Parâmetros de entrada</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabeçalho (Header)</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> é igual à autorização codificada por URL para o nome do usuário <strong>bx</strong> e a senha <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo para nome do usuário e senha do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: seu nome de usuário do {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`password`: sua senha do {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br><strong>nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</li></ul></td>
    </tr>
    <tr>
    <td>Corpo para chaves API do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: sua chave de API do {{site.data.keyword.Bluemix_notm}}</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</li></ul></td>
    </tr>
    <tr>
    <td>Corpo para senha única do {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: O {{site.data.keyword.Bluemix_notm}} senha descartável. Execute `ibmcloud login --sso` e siga as instruções em sua saída da CLI para recuperar sua senha descartável usando o navegador da web.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    Saída de exemplo para usar uma chave de API:

    ```
    {
    "access_token": "<iam_access_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    "scope": "ibm openid"
    }

    ```
    {: screen}

    É possível localizar o token do {{site.data.keyword.Bluemix_notm}} IAM no campo **access_token** de sua saída da API. Anote o token do {{site.data.keyword.Bluemix_notm}} IAM para recuperar informações adicionais do cabeçalho nas próximas etapas.

2.  Recupere o ID da conta do {{site.data.keyword.Bluemix_notm}} com a qual deseja trabalhar. Substitua `<iam_access_token>` pelo token do {{site.data.keyword.Bluemix_notm}} IAM recuperado do campo **access_token** de sua saída de API na etapa anterior. Em sua saída de API, é possível localizar o ID de sua conta do {{site.data.keyword.Bluemix_notm}} no campo **resources.metadata.guid**.

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Parâmetros de entrada para obter o ID da conta do {{site.data.keyword.Bluemix_notm}} com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
    <caption>Parâmetros de entrada para obter o ID da conta do {{site.data.keyword.Bluemix_notm}}.</caption>
    <thead>
  	<th>Parâmetros de entrada</th>
  	<th>Values</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Cabeçalhos</td>
      <td><ul><li><code>Content-Type: application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    Saída de exemplo:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  Gere um novo token do {{site.data.keyword.Bluemix_notm}} IAM que inclua suas credenciais do {{site.data.keyword.Bluemix_notm}} e o ID da conta com a qual deseja trabalhar.

    Se você usar uma chave de API do {{site.data.keyword.Bluemix_notm}}, deverá usar o ID da conta do {{site.data.keyword.Bluemix_notm}} para a qual a chave de API foi criada. Para acessar clusters em outras contas, efetue login nessa conta e crie uma chave API do {{site.data.keyword.Bluemix_notm}} que seja baseada nessa conta.
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parâmetros de entrada para recuperar tokens do IAM com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
    <caption>Parâmetros de entrada para obter tokens do IAM.</caption>
    <thead>
        <th>Parâmetros de entrada</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabeçalho (Header)</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>Nota</strong>: <code>Yng6Yng=</code> é igual à autorização codificada por URL para o nome do usuário <strong>bx</strong> e a senha <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo para nome do usuário e senha do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: seu nome de usuário do {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`password`: sua senha do {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</li>
    <li>`bss_account`: o ID da conta do {{site.data.keyword.Bluemix_notm}} que você recuperou na etapa anterior.</li></ul>
    </td>
    </tr>
    <tr>
    <td>Corpo para chaves API do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: sua chave de API do {{site.data.keyword.Bluemix_notm}}.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</li>
    <li>`bss_account`: o ID da conta do {{site.data.keyword.Bluemix_notm}} que você recuperou na etapa anterior.</li></ul>
      </td>
    </tr>
    <tr>
    <td>Corpo para senha única do {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: sua senha do {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</li>
    <li>`bss_account`: o ID da conta do {{site.data.keyword.Bluemix_notm}} que você recuperou na etapa anterior.</li></ul></td>
    </tr>
    </tbody>
    </table>

    Saída de exemplo:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    É possível localizar o token do {{site.data.keyword.Bluemix_notm}} IAM no campo **access_token** e o token de atualização no campo **refresh_token** de sua saída de API.

4.  Liste as regiões disponíveis do {{site.data.keyword.containerlong_notm}} e selecione aquela na qual deseja trabalhar. Use o token de acesso e o token de atualização do IAM da etapa anterior para construir suas informações de cabeçalho.
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="Parâmetros de entrada para recuperar regiões do {{site.data.keyword.containerlong_notm}} com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
    <caption>Parâmetros de entrada para recuperar regiões do {{site.data.keyword.containerlong_notm}}.</caption>
    <thead>
    <th>Parâmetros de entrada</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabeçalho (Header)</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    Saída de exemplo:
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  Liste todos os clusters na região do {{site.data.keyword.containerlong_notm}} selecionada. Se desejar [executar solicitações de API do Kubernetes com relação ao seu cluster](#kube_api), certifique-se de anotar o **id** e a **região** de seu cluster.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="Parâmetros de entrada para trabalhar com a API do {{site.data.keyword.containerlong_notm}} com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
     <caption>Parâmetros de entrada para trabalhar com a API do {{site.data.keyword.containerlong_notm}}.</caption>
     <thead>
     <th>Parâmetros de entrada</th>
     <th>Values</th>
     </thead>
     <tbody>
     <tr>
     <td>Cabeçalho (Header)</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Revise a [documentação da API do {{site.data.keyword.containerlong_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://containers.cloud.ibm.com/global/swagger-global-api) para localizar uma lista de APIs suportadas.

<br />


## Trabalhando com seu cluster por meio da API do Kubernetes
{: #kube_api}

É possível usar a [API do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/using-api/api-overview/) para interagir com seu cluster no {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

As instruções a seguir requerem acesso de rede pública em seu cluster para se conectar ao terminal de serviço público do principal de seu Kubernetes.
{: note}

1. Siga as etapas em [Automatizando implementações de cluster com a API](#cs_api) para recuperar o token de acesso de seu {{site.data.keyword.Bluemix_notm}} IAM, o token de atualização, o ID do cluster no qual deseja executar solicitações de API do Kubernetes e a região do {{site.data.keyword.containerlong_notm}} na qual seu cluster está localizado.

2. Recupere um token de atualização delegado do {{site.data.keyword.Bluemix_notm}} IAM.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Parâmetros de entrada para obter um token de atualização delegado do IAM com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
   <caption>Parâmetros de entrada para obter um token de atualização delegado do IAM. </caption>
   <thead>
   <th>Parâmetros de entrada</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabeçalho (Header)</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`</br><strong>Nota</strong>: <code>Yng6Yng=</code> é igual à autorização codificada por URL para o nome de usuário <strong>bx</strong> e a senha <strong>bx</strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Corpo</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token`: seu token de atualização do {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Saída de exemplo:
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. Recupere um token de ID, de acesso e de atualização do {{site.data.keyword.Bluemix_notm}} IAM usando o token de atualização delegado da etapa anterior. Em sua saída de API, é possível localizar o token do ID do IAM no campo **id_token**, o token de acesso do IAM no campo **access_token** e o token de atualização do IAM no campo **refresh_token**.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Parâmetros de entrada para obter os tokens de ID e acesso do IAM com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
   <caption>Parâmetros de entrada para obter os tokens de ID e acesso do IAM.</caption>
   <thead>
   <th>Parâmetros de entrada</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabeçalho (Header)</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic a3ViZTprdWJl`</br><strong>Nota</strong>: <code>a3ViZTprdWJl</code> é igual à autorização codificada por URL para o nome de usuário <strong><code>kube</code></strong> e a senha <strong><code>kube</code></strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Corpo</td>
   <td><ul><li>`refresh_token`: seu token de atualização delegado do {{site.data.keyword.Bluemix_notm}} IAM. </li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Saída de exemplo:
   ```
   {
    "access_token": "<iam_access_token>",
    "id_token": "<iam_id_token>",
    "refresh_token": "<iam_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. Recupere a URL pública do principal de seu Kubernetes usando o token de acesso, de ID e de atualização do IAM e a região do {{site.data.keyword.containerlong_notm}} na qual seu cluster está. É possível localizar a URL no **`publicServiceEndpointURL`** de sua saída de API.
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="Parâmetros de entrada para obter o terminal de serviço público para o principal de seu Kubernetes com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
   <caption>Parâmetros de entrada para obter o terminal de serviço público para o principal de seu Kubernetes.</caption>
   <thead>
   <th>Parâmetros de entrada</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabeçalho (Header)</td>
     <td><ul><li>`Authorization`: seu token de acesso do {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Auth-Refresh-Token`: seu token de atualização do {{site.data.keyword.Bluemix_notm}} IAM.</li><li>`X-Region`: a região do {{site.data.keyword.containerlong_notm}} de seu cluster recuperada com a API `GET https://containers.cloud.ibm.com/v1/clusters` em [Automatizando implementações de cluster com a API](#cs_api). </li></ul>
   </td>
   </tr>
   <tr>
   <td>Caminho</td>
   <td>`<cluster_ID>:` o ID de seu cluster recuperado com a API `GET https://containers.cloud.ibm.com/v1/clusters` em [Automatizando implementações de cluster com a API](#cs_api).      </td>
   </tr>
   </tbody>
   </table>

   Saída de exemplo:
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. Execute solicitações de API do Kubernetes com relação ao seu cluster usando o token de ID do IAM recuperado anteriormente. Por exemplo, liste a versão do Kubernetes executada em seu cluster.

   Se você tiver ativado a verificação de certificado SSL em sua estrutura de teste de API, certifique-se de desativar esse recurso.
   {: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="Parâmetros de entrada para visualizar a versão do Kubernetes executada em seu cluster com o parâmetro de entrada na coluna 1 e o valor na coluna 2. ">
   <caption>Parâmetros de entrada para visualizar a versão do Kubernetes executada em seu cluster. </caption>
   <thead>
   <th>Parâmetros de entrada</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Cabeçalho (Header)</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>Caminho</td>
   <td>`<publicServiceEndpointURL>`: o **`publicServiceEndpointURL`** do principal de seu Kubernetes recuperado na etapa anterior.      </td>
   </tr>
   </tbody>
   </table>

   Saída de exemplo:
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. Revise a [Documentação da API do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubernetes-api/) para localizar uma lista de APIs suportadas para a versão mais recente do Kubernetes. Certifique-se de usar a documentação da API que corresponda à versão do Kubernetes de seu cluster. Se você não usar a versão mais recente do Kubernetes, anexe sua versão no final da URL. Por exemplo, para acessar a documentação da API para a versão 1.12, inclua `v1.12`.


## Atualizando os tokens de acesso do {{site.data.keyword.Bluemix_notm}} IAM e obtendo novos tokens de atualização com a API
{: #cs_api_refresh}

Cada token de acesso do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) que é emitido por meio da API expira após uma hora. Deve-se atualizar seu token de acesso regularmente para assegurar acesso à
API do {{site.data.keyword.Bluemix_notm}}. É possível usar as mesmas etapas para obter um novo token de atualização.
{:shortdesc}

Antes de iniciar, certifique-se de que você tenha um token de atualização do {{site.data.keyword.Bluemix_notm}} IAM ou uma chave de API do {{site.data.keyword.Bluemix_notm}} que possa ser usada para solicitar um novo token de acesso.
- **Token de atualização:** siga as instruções em [Automatizando a criação do cluster e o processo de gerenciamento com a {{site.data.keyword.Bluemix_notm}}API do ](#cs_api).
- **Chave API:** Recupere sua chave API do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/) conforme a seguir.
   1. A partir da barra de menus, clique em **Gerenciar** > **Acesso (IAM)**.
   2. Clique na página **Usuários** e, em seguida, selecione você mesmo.
   3. Na área de janela **Chaves de API**, clique em **Criar uma chave de API do IBM Cloud**.
   4. Insira um **Nome** e uma **Descrição** para sua chave API e clique em **Criar**.
   4. Clique em **Mostrar** para ver a chave API que foi gerado para você.
   5. Copie a chave de API para que seja possível usá-la para recuperar seu novo token de acesso do {{site.data.keyword.Bluemix_notm}} IAM.

Use as etapas a seguir se você desejar criar um token do {{site.data.keyword.Bluemix_notm}} IAM ou se desejar obter um novo token de atualização.

1.  Gere um novo token de acesso do {{site.data.keyword.Bluemix_notm}} IAM usando o token de atualização ou a chave de API do {{site.data.keyword.Bluemix_notm}}.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parâmetros de entrada para o novo token do IAM com o parâmetro de entrada na coluna 1 e o valor na coluna 2.">
    <caption>Parâmetros de entrada para um novo token do {{site.data.keyword.Bluemix_notm}} IAM</caption>
    <thead>
    <th>Parâmetros de entrada</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabeçalho (Header)</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>Nota:</strong> <code>Yng6Yng=</code> é igual à autorização codificada por URL para o nome de usuário <strong>bx</strong> e a senha <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Corpo ao usar o token de atualização</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` seu token de atualização do {{site.data.keyword.Bluemix_notm}} IAM. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` seu ID da conta do {{site.data.keyword.Bluemix_notm}}. </li></ul><strong>Nota</strong>: inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    <tr>
      <td>Corpo ao usar a chave API do {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` sua chave de API do {{site.data.keyword.Bluemix_notm}}. </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>Nota:</strong> inclua a chave <code>uaa_client_secret</code> sem valor especificado.</td>
    </tr>
    </tbody>
    </table>

    Exemplo de saída da API:

    ```
    {
      "access_token": "<iam_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    }

    ```
    {: screen}

    É possível localizar seu novo token do {{site.data.keyword.Bluemix_notm}} IAM no **access_token** e o token de atualização no campo **refresh_token** de sua saída da API.

2.  Continue trabalhando com a [documentação da API do {{site.data.keyword.containerlong_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://containers.cloud.ibm.com/global/swagger-global-api) usando o token da etapa anterior.

<br />


## Atualizando os tokens de acesso do {{site.data.keyword.Bluemix_notm}} IAM e obtendo novos tokens de atualização com a CLI
{: #cs_cli_refresh}

Quando você inicia uma nova sessão da CLI ou se 24 horas expiraram em sua sessão da CLI atual, deve-se configurar o contexto para seu cluster executando `ibmcloud ks cluster-config --cluster <cluster_name>`. Quando você configura o contexto para seu cluster com esse comando, o arquivo `kubeconfig` para seu cluster do Kubernetes é transferido por download. Além disso, um token de ID do {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) e um token de atualização são emitidos para fornecer autenticação.
{: shortdesc}

**Token de ID**: cada token de ID do IAM que é emitido por meio da CLI expira após uma hora. Quando o token de ID expira, o token de atualização é enviado para o provedor de token para atualizar o token de ID. Sua autenticação é atualizada e é possível continuar a executar comandos com relação ao seu cluster.

**Token de atualização**: os tokens de atualização expiram a cada 30 dias. Quando o token de atualização expira, o token de ID não pode ser atualizado e não é possível continuar executando os comandos na CLI. É possível obter um novo token de atualização executando `ibmcloud ks cluster-config --cluster <cluster_name>`. Esse comando também atualiza seu token de ID.
