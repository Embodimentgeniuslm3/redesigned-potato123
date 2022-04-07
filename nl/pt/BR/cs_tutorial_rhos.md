---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, oks, iro, openshift, red hat, red hat openshift, rhos

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

# Tutorial: criando um cluster do Red Hat OpenShift on IBM Cloud (beta)
{: #openshift_tutorial}

O Red Hat OpenShift on IBM Cloud está disponível como um beta para testar clusters do OpenShift. Nem todos os recursos do {{site.data.keyword.containerlong}} estão disponíveis durante o beta. Além disso, quaisquer clusters beta do OpenShift que você cria permanecem por somente 30 dias depois que o beta termina e o Red Hat OpenShift on IBM Cloud se torna geralmente disponível.
{: preview}

Com o **Beta do Red Hat OpenShift on IBM Cloud**, é possível criar clusters do {{site.data.keyword.containerlong_notm}} com nós do trabalhador que vêm instalados com o software da plataforma de orquestração de contêiner do OpenShift. Você obtém todas as [vantagens do {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-responsibilities_iks) gerenciado para seu ambiente de infraestrutura de cluster ao usar o [conjunto de ferramentas e o catálogo do OpenShift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/welcome/index.html) que são executados no Red Hat Enterprise Linux para suas implementações de app.
{: shortdesc}

Os nós do trabalhador do OpenShift estão disponíveis somente para clusters padrão. O Red Hat OpenShift on IBM Cloud suporta o OpenShift versão 3.11 somente, o que inclui o Kubernetes versão 1.11.
{: note}

## Objetivos
{: #openshift_objectives}

Nas lições do tutorial, você cria um cluster padrão do Red Hat OpenShift on IBM Cloud, abre o console do OpenShift, acessa componentes integrados do OpenShift, implementa um app que usa serviços do {{site.data.keyword.Bluemix_notm}} em um projeto do OpenShift e expõe o app em uma rota do OpenShift para que os usuários externos possam acessar o serviço.
{: shortdesc}

Essa página também inclui informações sobre a arquitetura de cluster do OpenShift, as limitações do beta e como fornecer feedback e obter suporte.

## Tempo Necessário
{: #openshift_time}
45 minutos

## Público
{: #openshift_audience}

Este tutorial é para administradores de cluster que desejam aprender como criar um cluster do Red Hat OpenShift on IBM Cloud pela primeira vez.
{: shortdesc}

## Pré-requisitos
{: #openshift_prereqs}

*   Assegure-se de que você tenha as políticas de acesso do {{site.data.keyword.Bluemix_notm}} IAM a seguir.
    *   A [função da plataforma **Administrador**](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.containerlong_notm}}
    *   A [função do serviço **Gravador** ou **Gerenciador**](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.containerlong_notm}}
    *   A [função da plataforma **Administrador**](/docs/containers?topic=containers-users#platform) para o {{site.data.keyword.registrylong_notm}}
*    Certifique-se de que a [chave de API](/docs/containers?topic=containers-users#api_key) para a região e o grupo de recursos do {{site.data.keyword.Bluemix_notm}} esteja configurada com as permissões corretas de infraestrutura, com **Superusuário** ou com as [funções mínimas](/docs/containers?topic=containers-access_reference#infra) para criar um cluster.
*   Instale as ferramentas de linha de comandos.
    *   [Instale a CLI do {{site.data.keyword.Bluemix_notm}} (`ibmcloud`), o plug-in do {{site.data.keyword.containershort_notm}} (`ibmcloud ks`) e o plug-in do {{site.data.keyword.registryshort_notm}} (`ibmcloud cr`)](/docs/containers?topic=containers-cs_cli_install#cs_cli_install_steps).
    *   [Instale as CLIs do OpenShift Origin (`oc`) e do Kubernetes (`kubectl`)](/docs/containers?topic=containers-cs_cli_install#cli_oc).

<br />


## Visão geral arquitetural
{: #openshift_architecture}

O diagrama a seguir e a tabela descrevem os componentes padrão que são configurados em uma arquitetura do Red Hat OpenShift on IBM Cloud.
{: shortdesc}

![Arquitetura de cluster do Red Hat OpenShift on IBM Cloud](images/cs_org_ov_both_ses_rhos.png)

| Componentes principais| Descrição |
|:-----------------|:-----------------|
| Réplicas | Os componentes principais, incluindo o servidor da API do OpenShift Kubernetes e o armazenamento de dados etcd, têm três réplicas e, se localizados em uma área metropolitana de multizona, são difundidos entre as zonas para disponibilidade ainda mais alta. Os componentes principais são submetidos a backup a cada 8 horas.|
| `rhos-api` | O servidor da API do OpenShift Kubernetes serve como o ponto de entrada principal para todas as solicitações de gerenciamento de cluster do nó do trabalhador para o principal. O servidor da API valida e processa solicitações que mudam o estado de recursos do Kubernetes, como pods ou serviços, e armazena esse estado no armazenamento de dados etcd.|
| `openvpn-server` | O servidor OpenVPN funciona com o cliente OpenVPN para conectar com segurança o mestre ao nó do trabalhador. Essa conexão suporta chamadas `apiserver proxy` para seus pods e serviços e chamadas `kubectl exec`, `attach` e `logs` para o kubelet.|
| `etcd` | etcd é um armazenamento de valores de chaves altamente disponível que armazena o estado de todos os recursos do Kubernetes de um cluster, como serviços, implementações e pods. Os dados em etcd são submetidos a backup para uma instância de armazenamento criptografada que a IBM gerencia.|
| `rhos-controller` | O gerenciador do controlador do OpenShift observa os pods recém-criados e decide onde implementá-los com base na capacidade, necessidades de desempenho, restrições de política, especificações de antiafinidade e requisitos de carga de trabalho. Se não puder ser localizado nenhum nó do trabalhador que corresponda aos requisitos, o pod não será implementado
no cluster. O controlador também observa o estado de recursos de cluster, como conjuntos de réplicas. Quando o estado de um recurso for alterado, por exemplo, se um pod em um conjunto de réplicas ficar inativo, o gerenciador do controlador iniciará as ações de correção para atingir o estado necessário. O `rhos-controller` funciona como o planejador e o gerenciador do controlador em uma configuração nativa do Kubernetes. |
| `cloud-controller-manager` | O gerenciador do controlador de nuvem gerencia componentes específicos do provedor em nuvem, como o balanceador de carga do {{site.data.keyword.Bluemix_notm}}.|
{: caption="Tabela 1. Componentes principais do OpenShift." caption-side="top"}
{: #rhos-components-1}
{: tab-title="Master"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

| Componentes do nó do trabalhador| Descrição |
|:-----------------|:-----------------|
| Sistema operacional | Os nós do trabalhador do Red Hat OpenShift on IBM Cloud são executados no sistema operacional Red Hat Enterprise Linux 7 (RHEL 7). |
| Projetos | O OpenShift organiza seus recursos em projetos, que são namespaces do Kubernetes com anotações e inclui muito mais componentes do que clusters nativos do Kubernetes para executar recursos do OpenShift, como o catálogo. Os componentes selecionados de projetos são descritos nas linhas a seguir. Para obter mais informações, consulte [Projetos e usuários ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/architecture/core_concepts/projects_and_users.html).|
| `kube-system` | Esse namespace inclui vários componentes que são usados para executar o Kubernetes no nó do trabalhador.<ul><li>**`ibm-master-proxy`**: o `ibm-master-proxy` é um conjunto de daemon que encaminha as solicitações do nó do trabalhador para os endereços IP das réplicas principais altamente disponíveis. Em clusters de zona única, o principal tem três réplicas em hosts separados. Para clusters que estão em uma zona com capacidade para várias zonas, o mestre tem três réplicas que são difundidas entre as zonas. Um balanceador de carga altamente disponível encaminha as solicitações para o nome de domínio principal para as réplicas principais.</li><li>**`openvpn-client`**: o cliente OpenVPN trabalha com o servidor OpenVPN para conectar de forma segura o principal ao nó do trabalhador. Essa conexão suporta chamadas `apiserver proxy` para seus pods e serviços e chamadas `kubectl exec`, `attach` e `logs` para o kubelet.</li><li>**`kubelet`**: o kubelet é um agente do nó do trabalhador que é executado em cada nó trabalhador e é responsável por monitorar o funcionamento de pods que são executados no nó do trabalhador e por observar os eventos que o servidor da API do Kubernetes envia. Com base nos eventos, o kubelet cria ou remove pods, assegura as análises de vivacidade e prontidão e relata de volta o status dos pods para o servidor de API do Kubernetes.</li><li>**`calico`**: o Calico gerencia políticas de rede para seu cluster e inclui alguns componentes para gerenciar a conectividade de rede do contêiner, a designação de endereço IP e o controle de tráfego de rede.</li><li>**Outros componentes**: o namespace `kube-system` também inclui componentes para gerenciar recursos fornecidos pela IBM, tais como plug-ins de armazenamento para armazenamento de arquivo e bloco, balanceador de carga do aplicativo (ALB) do Ingress, criação de log do `fluentd` e `keepalived`.</li></ul>|
| `ibm-system` | Esse namespace inclui a implementação `ibm-cloud-provider-ip` que trabalha com `keepalived` para fornecer a verificação de funcionamento e o balanceamento de carga da Camada 4 para solicitações para os pods de app.|
| `kube-proxy-and-dns`| Esse namespace inclui os componentes para validar o tráfego de rede recebido com relação às regras `iptables` que estão configuradas no nó do trabalhador e as solicitações de proxies que podem entrar ou sair do cluster.|
| `default` | Esse namespace será usado se você não especificar um namespace ou criar um projeto para seus recursos do Kubernetes. Além disso, o namespace padrão inclui os componentes a seguir para suportar seus clusters do OpenShift.<ul><li>**`router`**: o OpenShift usa [rotas ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html) para expor o serviço de um app em um nome de host para que os clientes externos possam acessar o serviço. O roteador mapeia o serviço para o nome do host.</li><li>**`docker-registry`** e **`registry-console`**: o OpenShift fornece um [registro de imagem de contêiner ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/install_config/registry/index.html) interno que pode ser usado para gerenciar e visualizar localmente imagens por meio do console. Como alternativa, é possível configurar o {{site.data.keyword.registrylong_notm}} privado.</li></ul>|
| Outros projetos | Outros componentes são instalados em vários namespaces por padrão para ativar a funcionalidade, como criação de log, monitoramento e o console do OpenShift.<ul><li><code>ibm-cert-store</code></li><li><code>kube-public</code></li><li><code>kube-service-catalog</code></li><li><code>openshift</code></li><li><code>openshift-ansible-service-broker</code></li><li><code>openshift-console</code></li><li><code>openshift-infra</code></li><li><code>openshift-monitoring</code></li><li><code>openshift-node</code></li><li><code>openshift-template-service-broker</code></li><li><code>openshift-web-console</code></li></ul>|
{: caption="Tabela 2. Componentes do nó do trabalhador do OpenShift." caption-side="top"}
{: #rhos-components-2}
{: tab-title="Worker nodes"}
{: tab-group="rhos-components"}
{: class="simple-tab-table"}

<br />


## Lição 1: criando um cluster do Red Hat OpenShift on IBM Cloud
{: #openshift_create_cluster}

É possível criar um cluster do Red Hat OpenShift on IBM Cloud no {{site.data.keyword.containerlong_notm}} usando o [console](#openshift_create_cluster_console) ou a [CLI](#openshift_create_cluster_cli). Para aprender sobre quais componentes são configurados quando você cria um cluster, consulte a [Visão geral da arquitetura](#openshift_architecture). O OpenShift está disponível somente para clusters padrão. É possível aprender mais sobre o preço de clusters padrão nas [perguntas mais frequentes](/docs/containers?topic=containers-faqs#charges).
{:shortdesc}

É possível criar clusters somente no grupo de recursos **padrão**. Quaisquer clusters do OpenShift que você cria durante o beta permanecem por 30 dias depois que o beta termina e o Red Hat OpenShift on IBM Cloud se torna geralmente disponível.
{: important}

### Criando um cluster com o console
{: #openshift_create_cluster_console}

Crie um cluster padrão do OpenShift no console do {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

Antes de iniciar, [preencha os pré-requisitos](#openshift_prereqs) para certificar-se de que você tenha as permissões apropriadas para criar um cluster.

1.  Crie um cluster.
    1.  Efetue login em sua [conta do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/).
    2.  No menu de hambúrguer ![ícone do menu hambúrguer](../icons/icon_hamburger.svg "ícone do menu de hambúrguer"), selecione **Kubernetes** e, em seguida, clique em **Criar cluster**.
    3.  Escolha os detalhes e o nome da configuração do cluster. Para o beta, os clusters do OpenShift estão disponíveis somente como clusters padrão que estão localizados em data centers de Washington, DC e Londres.
        *   Para **Selecionar um plano**, escolha **Padrão**.
        *   Para **Grupo de recursos**, deve-se usar o **padrão**.
        *   Para o **Local**, configure a geografia para **América do Norte** ou **Europa**, selecione uma disponibilidade de **Zona única** ou **Multizona** e, em seguida, selecione as zonas do trabalhador **Washington, DC** ou **Londres**.
        *   Para **Conjunto de trabalhadores padrão**, selecione a versão do cluster do **OpenShift**. O Red Hat OpenShift on IBM Cloud suporta o OpenShift versão 3.11 somente, o que inclui o Kubernetes versão 1.11. Escolha um tipo disponível para seus nós de trabalhador idealmente com pelo menos RAM de 16 GB de quatro núcleos.
        *   Configure um número de nós do trabalhador para criar por zona, como 3.
    4.  Para concluir, clique em **Criar cluster**.<p class="note">A criação do cluster pode levar algum tempo para ser concluída. Depois que o estado do cluster mostra **Normal**, a rede de cluster e os componentes de balanceamento de carga levam mais de 10 minutos para implementar e atualizar o domínio do cluster usado para o console da web do OpenShift e outras rotas. Aguarde até que o cluster esteja pronto antes de continuar com a próxima etapa, verificando se o **Subdomínio do Ingress** segue um padrão de `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
2.  Na página de detalhes do cluster, clique em **Console da web do OpenShift**.
3.  No menu suspenso na barra de menus da plataforma de contêineres OpenShift, clique em **Console do aplicativo**. O Console do aplicativo lista todos os namespaces do projeto em seu cluster. É possível navegar para um namespace para visualizar seus aplicativos, construções e outros recursos do Kubernetes.
4.  Para concluir a próxima lição trabalhando no terminal, clique em seu perfil **IAM#user.name@email.com > Copiar comando de login**. Cole o comando de login `oc` copiado em seu terminal para autenticar por meio da CLI.

### Criando um cluster com a CLI
{: #openshift_create_cluster_cli}

Crie um cluster padrão do OpenShift usando a CLI do {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

Antes de iniciar, [preencha os pré-requisitos](#openshift_prereqs) para certificar-se de que você tenha as permissões apropriadas para criar um cluster, a CLI `ibmcloud` e os plug-ins e as CLIs `oc` e `kubectl`.

1.  Efetue login na conta que você configurou para criar clusters do OpenShift. Tenha como destino a região **us-east** ou **eu-gb** e o grupo de recursos **padrão**. Se você tiver uma conta federada, inclua a sinalização `--sso`.
    ```
    ibmcloud login -r (us-east|eu-gb) -g default [--sso]
    ```
    {: pre}
2.  Crie um cluster.
    ```
    ibmcloud ks cluster-create --name <name> --location <zone> --kube-version <openshift_version> --machine-type <worker_node_flavor> --workers <number_of_worker_nodes_per_zone> --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    Exemplo de comando para criar um cluster com três nós de trabalhadores que têm quatro núcleos e 16 GB de memória em Washington, DC.

    ```
    ibmcloud ks cluster-create --name my_openshift --location wdc04 --kube-version 3.11_openshift --machine-type b3c.4x16.encrypted  --workers 3 --public-vlan <public_VLAN_ID> --private-vlan <private_VLAN_ID>
    ```
    {: pre}

    <table summary="A primeira linha na tabela abrange ambas as colunas. O restante das linhas deve ser lido da esquerda para a direita, com o componente de comando na coluna um e a descrição correspondente na coluna dois.">
    <caption>Os componentes de criação de cluster</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>cluster-create</code></td>
    <td>O comando para criar um cluster de infraestrutura clássica em sua conta do {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--name <em>&lt;name&gt;</em></code></td>
    <td>Insira um nome para seu cluster. O nome deve iniciar com uma letra, pode conter letras, números e hífen (-) e deve ter 35 caracteres ou menos. Use um nome que seja exclusivo em regiões do {{site.data.keyword.Bluemix_notm}}.</td>
    </tr>
    <tr>
    <td><code>--location <em>&lt;zone&gt;</em></code></td>
    <td>Especifique a zona na qual você deseja criar seu cluster. Para o beta, as zonas disponíveis são `wdc04, wdc06, wdc07, lon04, lon05` ou `lon06`.</p></td>
    </tr>
    <tr>
      <td><code>--kube-version <em>&lt;openshift_version&gt;</em></code></td>
      <td>Deve-se escolher uma versão suportada do OpenShift. As versões do OpenShift incluem uma versão do Kubernetes que difere das versões do Kubernetes que estão disponíveis em clusters nativos do Kubernetes Ubuntu. Para listar as versões disponíveis do OpenShift, execute `ibmcloud ks versions`. Para criar um cluster com a versão de correção mais recente, é possível especificar apenas a versão principal e secundária, tal como ` 3.11_openshift`.<br><br>O Red Hat OpenShift on IBM Cloud suporta o OpenShift versão 3.11 somente, o que inclui o Kubernetes versão 1.11.</td>
    </tr>
    <tr>
    <td><code>--machine-type <em>&lt;worker_node_flavor&gt;</em></code></td>
    <td>Escolha um tipo de máquina. É possível implementar os nós do trabalhador como máquinas virtuais em hardware compartilhado ou dedicado ou como máquinas físicas no bare metal. Os tipos de máquinas físicas e virtuais disponíveis variam de acordo com a zona na qual você implementa o cluster. Para listar os tipos de máquina disponíveis, execute `ibmcloud ks machine-types --zone <zone>`.</td>
    </tr>
    <tr>
    <td><code>--workers <em>&lt;number_of_worker_nodes_per_zone&gt;</em></code></td>
    <td>O número de nós do trabalhador a serem incluídos no cluster. Você pode desejar especificar pelo menos três nós do trabalhador para que seu cluster tenha recursos suficientes para executar os componentes padrão e para alta disponibilidade. Se a opção <code>--workers</code> não for especificada, um nó do trabalhador será criado.</td>
    </tr>
    <tr>
    <td><code>--public-vlan <em>&lt;public_vlan_id&gt;</em></code></td>
    <td>Se você já tiver uma VLAN pública configurada em sua conta de infraestrutura do IBM Cloud (SoftLayer) para essa zona, insira o ID da VLAN pública. Para verificar as VLANs disponíveis, execute `ibmcloud ks vlans --zone <zone>`. <br><br>Se você não tiver uma VLAN pública em sua conta, não especifique essa opção. O {{site.data.keyword.containerlong_notm}} cria automaticamente uma VLAN pública para você.</td>
    </tr>
    <tr>
    <td><code>--private-vlan <em>&lt;private_vlan_id&gt;</em></code></td>
    <td>Se você já tiver uma VLAN privada configurada em sua conta de infraestrutura do IBM Cloud (SoftLayer) para essa zona, insira o ID da VLAN privada. Para verificar as VLANs disponíveis, execute `ibmcloud ks vlans --zone <zone>`. <br><br>Se você não tem uma VLAN privada em sua conta, não especifique esta opção. O {{site.data.keyword.containerlong_notm}} cria automaticamente uma VLAN privada para você.</td>
    </tr>
    </tbody></table>
3.  Liste os detalhes do cluster. Revise o **Estado** do cluster, verifique o **Subdomínio do Ingress** e anote a **URL principal**.<p class="note">A criação do cluster pode levar algum tempo para ser concluída. Depois que o estado do cluster mostra **Normal**, a rede de cluster e os componentes de balanceamento de carga levam mais de 10 minutos para implementar e atualizar o domínio do cluster usado para o console da web do OpenShift e outras rotas. Aguarde até que o cluster esteja pronto antes de continuar com a próxima etapa, verificando se o **Subdomínio do Ingress** segue um padrão de `<cluster_name>.<region>.containers.appdomain.cloud`.</p>
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
4.  Faça download dos arquivos de configuração para se conectar ao seu cluster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Quando o download dos arquivos de configuração é concluído, é exibido um comando que pode ser copiado e colado para configurar o caminho para o arquivo de configuração do Kubernetes local como uma variável de ambiente.

    Exemplo para OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
5.  Em seu navegador, navegue para o endereço de sua **URL principal** e anexe `/console`. Por exemplo, `https://c0.containers.cloud.ibm.com:23652/console`.
6.  Clique em seu perfil **IAM#user.name@email.com > Copiar comando de login**. Cole o comando de login `oc` copiado em seu terminal para autenticar por meio da CLI.<p class="tip">Salve a URL do cluster principal para acessar o console do OpenShift posteriormente. Em sessões futuras, será possível ignorar a etapa `cluster-config` e copiar o comando de login por meio do console.</p>
7.  Verifique se os comandos `oc` são executados corretamente com seu cluster verificando a versão.

    ```
    oc version
    ```
    {: pre}

    Saída de exemplo:

    ```
    oc v3.11.0+0cbc58b
    kubernetes v1.11.0+d4cacc0
    features: Basic-Auth

    Server https://c0.containers.cloud.ibm.com:23652
    openshift v3.11.98
    kubernetes v1.11.0+d4cacc0
    ```
    {: screen}

    Se não for possível executar operações que requerem permissões de Administrador, como listar todos os nós do trabalhador ou os pods em um cluster, faça download dos certificados TLS e dos arquivos de permissão para o administrador do cluster executando o comando `ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin`.
    {: tip}

<br />


## Lição 2: acessando serviços integrados do OpenShift
{: #openshift_access_oc_services}

O Red Hat OpenShift on IBM Cloud é fornecido com serviços integrados que podem ser usados para ajudar a operar seu cluster, como o console do OpenShift, o Prometheus e o Grafana. Para o beta, para acessar esses serviços, é possível usar o host local de uma [rota ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/architecture/networking/routes.html). Os nomes de domínio de rota padrão seguem um padrão específico do cluster de `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
{:shortdesc}

É possível acessar as rotas de serviço integrado do OpenShift por meio do console do [console](#openshift_services_console) ou da [CLI](#openshift_services_cli). Você pode desejar usar o console para navegar por recursos do Kubernetes em um projeto. Usando a CLI, é possível listar recursos como rotas em projetos.

### Acessando serviços integrados do OpenShift por meio do console
{: #openshift_services_console}
1.  No console da web do OpenShift, no menu suspenso na barra de menus da plataforma de contêineres OpenShift, clique em **Console do aplicativo**.
2.  Selecione o projeto **padrão**, em seguida, na área de janela de navegação, clique em **Aplicativos > Pods**.
3.  Verifique se os pods de **roteador** estão em um status **Em execução**. O roteador funciona como o ponto de ingresso para o tráfego de rede externo. É possível usar o roteador para expor publicamente os serviços em seu cluster no endereço IP externo do roteador usando uma rota. O roteador atende na interface de rede do host público, ao contrário de seus pods de app que atendem apenas em IPs privados. O roteador transmite por proxy solicitações externas para rotear nomes de host para os IPs dos pods do app que são identificados pelo serviço que você associou ao nome do host da rota.
4.  Na área de janela de navegação do projeto **padrão**, clique em **Aplicativos > Implementações** e, em seguida, clique na implementação **registry-console**. Para abrir o console de registro interno, deve-se atualizar a URL do provedor para que seja possível acessá-la externamente.
    1.  Na guia **Ambiente** da página de detalhes do **registry-console**, localize o campo **OPENSHIFT_OAUTH_PROVIDER_URL**. 
    2. No campo de valor, inclua `-e` após o `c1`, como em `https://c1-e.eu-gb.containers.cloud.ibm.com:20399`. 
    3. Clique em **Salvar**. Agora, a implementação do console de registro pode ser acessada por meio do terminal de API pública do cluster principal.
    4.  Na área de janela de navegação do projeto **padrão**, clique em **Aplicativos > Rotas**. Para abrir o console de registro, clique no valor de **Nome do host**, como `https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">Para o beta, o console de registro usa certificados TLS autoassinados, portanto, deve-se escolher continuar para chegar ao console de registro. No Google Chrome, clique em **Avançado > Continuar com <cluster_master_URL>**. Outros navegadores têm opções semelhantes. Se não for possível continuar com essa configuração, tente abrir a URL em um navegador privado.</p>
5.  Na barra de menus da plataforma de contêineres OpenShift, no menu suspenso, clique em **Console do cluster**.
6.  Na área de janela de navegação, expanda **Monitoramento**.
7.  Clique na ferramenta de monitoramento integrada que você deseja acessar, tal como **Painéis**. A rota do Grafana é aberta, `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.<p class="note">Na primeira vez que você acessar o nome do host poderá ser necessário se autenticar, como clicar em **Efetuar login com o OpenShift** e autorizar o acesso à sua identidade do IAM.</p>

### Acessando serviços integrados do OpenShift por meio da CLI
{: #openshift_services_cli}

1.  No console da web do OpenShift, clique em seu perfil **IAM#user.name@email.com > Copiar comando de login** e cole o comando de login em seu terminal para autenticar.
    ```
    oc login https://c1-e.<region>.containers.cloud.ibm.com:<port> --token=<access_token>
    ```
    {: pre}
2.  Verifique se o roteador está implementado. O roteador funciona como o ponto de ingresso para o tráfego de rede externo. É possível usar o roteador para expor publicamente os serviços em seu cluster no endereço IP externo do roteador usando uma rota. O roteador atende na interface de rede do host público, ao contrário de seus pods de app que atendem apenas em IPs privados. O roteador transmite por proxy solicitações externas para rotear nomes de host para os IPs dos pods do app que são identificados pelo serviço que você associou ao nome do host da rota.
    ```
    oc get svc router -n default
    ```
    {: pre}

    Saída de exemplo:
    ```
    NAME      TYPE           CLUSTER-IP               EXTERNAL-IP     PORT(S)                      AGE
    router    LoadBalancer   172.21.xxx.xxx   169.xx.xxx.xxx   80:30399/TCP,443:32651/TCP                      5h
    ```
    {: screen}
2.  Obtenha o nome do host **Host/Porta** da rota de serviço que você deseja acessar. Por exemplo, você talvez deseje acessar o painel Grafana para verificar as métricas no uso de recurso de seu cluster. Os nomes de domínio de rota padrão seguem um padrão específico do cluster de `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`.
    ```
    oc get route --all-namespaces
    ```
    {: pre}

    Saída de exemplo:
    ```
    NAMESPACE                          NAME                HOST/PORT                                                                    PATH                  SERVICES            PORT               TERMINATION          WILDCARD
    default                            registry-console    registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                              registry-console    registry-console   passthrough          None
    kube-service-catalog               apiserver           apiserver-kube-service-catalog.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                        apiserver           secure             passthrough          None
    openshift-ansible-service-broker   asb-1338            asb-1338-openshift-ansible-service-broker.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud            asb                 1338               reencrypt            None
    openshift-console                  console             console.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                                              console             https              reencrypt/Redirect   None
    openshift-monitoring               alertmanager-main   alertmanager-main-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                alertmanager-main   web                reencrypt            None
    openshift-monitoring               grafana             grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                          grafana             https              reencrypt            None
    openshift-monitoring               prometheus-k8s      prometheus-k8s-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud                   prometheus-k8s      web                reencrypt
    ```
    {: screen}
3.  **Atualização única do registro**: para tornar seu console de registro interno acessível na Internet, edite a implementação `registry-console` para usar o terminal de API pública de seu cluster mestre como a URL do provedor do OpenShift. O terminal de API pública tem o mesmo formato que o terminal de API privada, mas inclui um `-e` adicional na URL.
    ```
    oc edit deploy registry-console -n default
    ```
    {: pre}
    
    No campo `Pod Template.Containers.registry-console.Environment.OPENSHIFT_OAUTH_PROVIDER_URL`, inclua `-e` após o `c1`, como em `https://ce.eu-gb.containers.cloud.ibm.com:20399`.
    ```
    Name:                   registry-console
    Namespace:              default
    ...
    Pod Template:
      Labels:  name=registry-console
      Containers:
       registry-console:
        Image:      registry.eu-gb.bluemix.net/armada-master/iksorigin-registrconsole:v3.11.98-6
        ...
        Environment:
          OPENSHIFT_OAUTH_PROVIDER_URL:  https://c1-e.eu-gb.containers.cloud.ibm.com:20399
          ...
    ```
    {: screen}
4.  Em seu navegador da web, abra a rota que você deseja acessar, por exemplo: `https://grafana-openshift-monitoring.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. Na primeira vez que você acessar o nome do host poderá ser necessário se autenticar, como clicar em **Efetuar login com o OpenShift** e autorizar o acesso à sua identidade do IAM.

<br>
Agora você está no app OpenShift integrado! Por exemplo, se você estiver no Grafana, poderá consultar o uso de CPU do namespace ou de outros gráficos. Para acessar outras ferramentas integradas, abra seus nomes de host de rota.

<br />


## Lição 3: implementando um app em seu cluster do OpenShift
{: #openshift_deploy_app}

Com o Red Hat OpenShift on IBM Cloud, é possível criar um novo app e expor seu serviço de app por meio de um roteador do OpenShift para usuários externos usarem.
{: shortdesc}

Se você fez uma pausa na última lição e iniciou um novo terminal, certifique-se de efetuar login novamente em seu cluster. Abra seu console do OpenShift em `https://<master_URL>/console`. Por exemplo, `https://c0.containers.cloud.ibm.com:23652/console`. Em seguida, clique em seu perfil **IAM#user.name@email.com > Copiar comando de login** e cole o comando de login `oc` copiado em seu terminal para autenticar por meio da CLI.
{: tip}

1.  Crie um projeto para seu app Hello World. Um projeto é uma versão do OpenShift de um namespace do Kubernetes com anotações adicionais.
    ```
    oc new-project hello-world
    ```
    {: pre}
2.  Construa o aplicativo de amostra [por meio do código-fonte ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/IBM/container-service-getting-started-wt). Com o comando `new-app` do OpenShift, é possível referir-se a um diretório em um repositório remoto que contenha o Dockerfile e o código do app para construir sua imagem. O comando constrói a imagem, armazena a imagem no registro do Docker local e cria as configurações de implementação de app (`dc`) e os serviços (`svc`). Para obter mais informações sobre a criação de novos apps, [consulte os docs do OpenShift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/dev_guide/application_lifecycle/new_app.html).
    ```
    oc new-app --name hello-world https://github.com/IBM/container-service-getting-started-wt --context-dir="Lab 1"
    ```
    {: pre}
3.  Verifique se os componentes do app Hello World de amostra são criados.
    1.  Verifique a imagem **hello-world** no registro do Docker integrado do cluster acessando o console de registro em seu navegador. Certifique-se de ter atualizado a URL do provedor do console de registro com `-e` conforme descrito na lição anterior.
        ```
        https://registry-console-default.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud/
        ```
        {: codeblock}
    2.  Liste os serviços **hello-world** e anote o nome do serviço. Seu app atende o tráfego nesses endereços IP do cluster interno, a menos que você crie uma rota para o serviço para que o roteador possa encaminhar solicitações de tráfego externo para o app.
        ```
        oc get svc -n hello-world
        ```
        {: pre}

        Saída de exemplo:
        ```
        NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
        hello-world   ClusterIP   172.21.xxx.xxx   <nones>       8080/TCP   31m
        ```
        {: screen}
    3.  Liste os pods. Os pods com `build` no nome são tarefas **Concluídas** como parte do novo processo de construção do app. Certifique-se de que o status do pod **hello-world** seja **Em execução**.
        ```
        oc get pods -n hello-world
        ```
        {: pre}

        Saída de exemplo:
        ```
        NAME                READY     STATUS             RESTARTS   AGE
        hello-world-9cv7d   1/1       Running            0          30m
        hello-world-build   0/1       Completed          0          31m
        ```
        {: screen}
4.  Configure uma rota para que seja possível acessar publicamente o serviço {{site.data.keyword.toneanalyzershort}}. Por padrão, o nome do host está no formato de `<service_name>-<namespace>.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud`. Se você desejar customizar o nome do host, inclua a sinalização `--hostname=<hostname>`.
    1.  Crie uma rota para o serviço **hello-world**.
        ```
        oc create route edge --service=hello-world -n hello-world
        ```
        {: pre}
    2.  Obtenha o endereço do nome do host da rota por meio da saída de **Host/Porta**.
        ```
        oc get route -n hello-world
        ```
        {: pre}
        Saída de exemplo:
        ```
        NAME          HOST/PORT                         PATH                                        SERVICES      PORT       TERMINATION   WILDCARD
        hello-world   hello-world-hello.world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud    hello-world   8080-tcp   edge/Allow    None
        ```
        {: screen}
5.  Acesse seu app.
    ```
    curl https://hello-world-hello-world.<cluster_name>-<random_ID>.<region>.containers.appdomain.cloud
    ```
    {: pre}
    
    Saída de exemplo:
    ```
    Hello world from hello-world-9cv7d! Seu app está funcionando em um contêiner!
    ```
    {: screen}
6.  **Opcional** Para limpar os recursos que você criou nesta lição, é possível usar os rótulos que são designados a cada app.
    1.  Liste todos os recursos para cada app no projeto `hello-world`.
        ```
        oc get all -l app=hello-world -o name -n hello-world
        ```
        {: pre}
        Saída de exemplo:
        ```
        pod/hello-world-1-dh2ff
        replicationcontroller/hello-world-1
        service/hello-world
        deploymentconfig.apps.openshift.io/hello-world
        buildconfig.build.openshift.io/hello-world
        build.build.openshift.io/hello-world-1
        imagestream.image.openshift.io/hello-world
        imagestream.image.openshift.io/node
        route.route.openshift.io/hello-world
        ```
        {: screen}
    2.  Exclua todos os recursos que você criou.
        ```
        oc delete all -l app=hello-world -n hello-world
        ```
        {: pre}



<br />


## Lição 4: configurando os complementos LogDNA e Sysdig para monitorar o funcionamento do cluster
{: #openshift_logdna_sysdig}

Como o OpenShift configura as [Restrições de contexto de segurança (SCC) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) mais estritas por padrão do que o Kubernetes nativo, você pode achar que alguns apps ou complementos de cluster usados no Kubernetes nativo não podem ser implementados no OpenShift da mesma maneira. Em particular, muitas imagens requerem a execução como um usuário `root` ou como um contêiner privilegiado, que é evitado no OpenShift por padrão. Nesta lição, você aprenderá como modificar os SCCs padrão, criando contas de segurança privilegiada e atualizando o `securityContext` na especificação de pod para usar dois complementos populares do {{site.data.keyword.containerlong_notm}}: {{site.data.keyword.la_full_notm}} e {{site.data.keyword.mon_full_notm}}.
{: shortdesc}

Antes de iniciar, efetue login em seu cluster como um administrador.
1.  Abra seu console do OpenShift em `https://<master_URL>/console`. Por exemplo, `https://c0.containers.cloud.ibm.com:23652/console`.
2.  Clique em seu perfil **IAM#user.name@email.com > Copiar comando de login** e cole o comando de login `oc` copiado em seu terminal para autenticar por meio da CLI.
3.  Faça download dos arquivos de configuração do administrador para seu cluster.
    ```
    ibmcloud ks cluster-config --cluster <cluster_name_or_ID> --admin
    ```
    {: pre}
    
    Quando o download dos arquivos de configuração é concluído, é exibido um comando que pode ser copiado e colado para configurar o caminho para o arquivo de configuração do Kubernetes local como uma variável de ambiente.

    Exemplo para OS X:

    ```
    export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/<cluster_name>/kube-config-<datacenter>-<cluster_name>.yml
    ```
    {: screen}
4.  Continue a lição para configurar o [{{site.data.keyword.la_short}}](#openshift_logdna) e o [{{site.data.keyword.mon_short}}](#openshift_sysdig).

### Lição 4a: configurando o LogDNA
{: #openshift_logdna}

Configure um projeto e uma conta de serviço privilegiado para o {{site.data.keyword.la_full_notm}}. Em seguida, crie uma instância do {{site.data.keyword.la_short}} em sua conta do {{site.data.keyword.Bluemix_notm}}. Para integrar sua instância do {{site.data.keyword.la_short}} ao seu cluster do OpenShift, deve-se modificar o conjunto de daemon que é implementado para usar a conta de serviço privilegiado para executar como raiz.
{: shortdesc}

1.  Configure o projeto e a conta de serviço privilegiado para o LogDNA.
    1.  Como um administrador de cluster, crie um projeto `logdna`.
        ```
        oc adm new-project logdna
        ```
        {: pre}
    2.  Tenha o projeto como destino para que os recursos subsequentes que você criar estejam no namespace do projeto `logdna`.
        ```
        oc project logdna
        ```
        {: pre}
    3.  Crie uma conta de serviço para o projeto `logdna`.
        ```
        oc create serviceaccount logdna
        ```
        {: pre}
    4.  Inclua uma restrição de contexto de segurança privilegiada para a conta de serviço para o projeto `logdna`.<p class="note">Se você desejar verificar qual autorização a política SCC `privileged` fornece à conta de serviço, execute `oc describe scc privileged`. Para obter mais informações sobre SCCs, consulte os [docs do OpenShift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).</p>
        ```
        oc adm policy add-scc-to-user privileged -n logdna -z logdna
        ```
        {: pre}
2.  Crie sua instância do {{site.data.keyword.la_full_notm}} no mesmo grupo de recursos que seu cluster. Selecione um plano de precificação que determina o período de retenção para seus logs, como `lite`, que retém logs por 0 dias. A região não tem que corresponder à região de seu cluster. Para obter mais informações, consulte [Provisionando uma instância](/docs/services/Log-Analysis-with-LogDNA/tutorials?topic=LogDNA-provision) e [Planos de precificação](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-about#overview_pricing_plans).
    ```
    ibmcloud resource service-instance-create <service_instance_name> logdna (lite|7-days|14-days|30-days) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Exemplo de comando:
    ```
    ibmcloud resource service-instance-create logdna-openshift logdna lite us-south
    ```
    {: pre}
    
    Na saída, anote o **ID** da instância de serviço, que está no formato `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    A instância de serviço <name> foi criada.
                 
    Nome: <name>   
    ID: crn:v1:bluemix:public:logdna:<region>:<ID_string>::   
    GUID: <guid>   
    Local: <region>   
    ...
    ```
    {: screen}    
3.  Obtenha sua chave de ingestão da instância do {{site.data.keyword.la_short}}. A chave de ingestão do LogDNA é usada para abrir um soquete seguro da web para o servidor de ingestão do LogDNA e para autenticar o agente de criação de log com o serviço {{site.data.keyword.la_short}}.
    1.  Crie uma chave de serviço para sua instância do LogDNA.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <logdna_instance_ID>
        ```
        {: pre}
    2.  Anote o **ingestion_key** de sua chave de serviço.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Saída de exemplo:
        ```
        Nome: <key_name>  
        ID: crn:v1:bluemix:public:logdna:<region>:<ID_string>::    
        Criado em: Thu Jun 6 21:31:25 UTC 2019   
        Estado: ativo   
        Credenciais:                                   
                       apikey: <api_key_value>      
                       iam_apikey_description: gerado automaticamente para a chave <ID>     
                       iam_apikey_name: <key_name>       
                       iam_role_crn: crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn: crn:v1:bluemix:public:iam-identity::<ID_string>     
                       ingestion_key:            111a11aa1a1a11a1a11a1111aa1a1a11    
        ```
        {: screen}
4.  Crie um segredo do Kubernetes para armazenar sua chave de ingestão do LogDNA para sua instância de serviço.
    ```
     oc create secret generic logdna-agent-key --from-literal=logdna-agent-key=<logDNA_ingestion_key>
    ```
    {: pre}
5.  Crie um conjunto de daemon do Kubernetes configurado para implementar o agente do LogDNA em cada nó do trabalhador de seu cluster Kubernetes. O agente LogDNA coleta logs com a extensão `*.log` e arquivos sem extensão que são armazenados no diretório `/var/log` do seu pod. Por padrão, os logs são coletados de todos os namespaces, incluindo `kube-system` e encaminhados automaticamente para o serviço do {{site.data.keyword.la_short}}.
    ```
    oc create -f https://assets.us-south.logging.cloud.ibm.com/clients/logdna-agent-ds.yaml
    ```
    {: pre}
6.  Edite a configuração do conjunto de daemon do agente LogDNA para referir-se à conta de serviço que você criou anteriormente e para configurar o contexto de segurança para privilegiado.
    ```
    oc edit ds logdna-agent
    ```
    {: pre}
    
    No arquivo de configuração, inclua as especificações a seguir.
    *   Em `spec.template.spec`, inclua `serviceAccount: logdna`.
    *   Em `spec.template.spec.containers`, inclua `securityContext: privileged: true`.
    *   Se você criou sua instância do {{site.data.keyword.la_short}} em uma região diferente de `us-south`, atualize os valores da variável de ambiente `spec.template.spec.containers.env` para o `LDAPIHOST` e `LDLOGHOST` com o `<region>`.

    Saída de exemplo:
    ```
    apiVersion: extensions/v1beta1
    kind: DaemonSet
    spec:
      ...
      :
        ...
        spec:
          serviceAccount: logdna
          containers:
          - securityContext:
              privileged: true
            ...
            env:
            - name: LOGDNA_AGENT_KEY
              valueFrom:
                secretKeyRef:
                  key: logdna-agent-key
                  name: logdna-agent-key
            - name: LDAPIHOST
              value: api.<region>.logging.cloud.ibm.com
            - name: LDLOGHOST
              value: logs.<region>.logging.cloud.ibm.com
          ...
    ```
    {: screen}
7.  Verifique se o pod `logdna-agent` em cada nó está em um status **Em execução**.
    ```
    oc get pods
    ```
    {: pre}
8.  Em [Observabilidade do {{site.data.keyword.Bluemix_notm}} > Console de criação de log](https://cloud.ibm.com/observe/logging), na linha para sua instância do {{site.data.keyword.la_short}}, clique em **Visualizar LogDNA**. O painel LogDNA é aberto e é possível começar a analisar seus logs.

Para obter mais informações sobre como usar o {{site.data.keyword.la_short}}, consulte os [docs de Próximas etapas](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-kube#kube_next_steps).

### Lição 4b: configurando o Sysdig
{: #openshift_sysdig}

Crie uma instância do {{site.data.keyword.mon_full_notm}} em sua conta do {{site.data.keyword.Bluemix_notm}}. Para integrar sua instância do {{site.data.keyword.mon_short}} ao seu cluster do OpenShift, deve-se executar um script que crie um projeto e uma conta de serviço privilegiado para o agente Sysdig.
{: shortdesc}

1.  Crie sua instância do {{site.data.keyword.mon_full_notm}} no mesmo grupo de recursos que seu cluster. Selecione um plano de precificação que determine o período de retenção para seus logs, como `lite`. A região não tem que corresponder à região de seu cluster. Para obter mais informações, consulte [Provisionando uma instância](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-provision).
    ```
    ibmcloud resource service-instance-create <service_instance_name> sysdig-monitor (lite|graduated-tier) <region> [-g <resource_group>]
    ```
    {: pre}
    
    Exemplo de comando:
    ```
    ibmcloud resource service-instance-create sysdig-openshift sysdig-monitor lite us-south
    ```
    {: pre}
    
    Na saída, anote o **ID** da instância de serviço, que está no formato `crn:v1:bluemix:public:logdna:<region>:<ID_string>::`.
    ```
    A instância de serviço <name> foi criada.
                 
    Nome: <name>   
    ID: crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::   
    GUID: <guid>   
    Local: <region>   
    ...
    ```
    {: screen}    
2.  Obtenha sua chave de acesso da instância do {{site.data.keyword.mon_short}}. A chave de acesso do Sysdig é usada para abrir um soquete seguro da web para o servidor de ingestão do Sysdig e para autenticar o agente de monitoramento com o serviço {{site.data.keyword.mon_short}}.
    1.  Crie uma chave de serviço para sua instância do Sysdig.
        ```
        ibmcloud resource service-key-create <key_name> Administrator --instance-id <sysdig_instance_ID>
        ```
        {: pre}
    2.  Anote a **Chave de acesso do Sysdig** e o **Terminal do coletor do Sysdig** de sua chave de serviço.
        ```
        ibmcloud resource service-key <key_name>
        ```
        {: pre}
        
        Saída de exemplo:
        ```
        Nome: <key_name>  
        ID: crn:v1:bluemix:public:sysdig-monitor:<region>:<ID_string>::    
        Criado em: Thu Jun 6 21:31:25 UTC 2019   
        Estado: ativo   
        Credenciais:                                   
                       Sysdig Access Key:           11a1aa11-aa1a-1a1a-a111-11a11aa1aa11      
                       Sysdig Collector Endpoint:   ingest.<region>.monitoring.cloud.ibm.com      
                       Sysdig Customer Id:          11111      
                       Sysdig Endpoint:             https://<region>.monitoring.cloud.ibm.com  
                       apikey: <api_key_value>      
                       iam_apikey_description: gerado automaticamente para a chave <ID>     
                       iam_apikey_name: <key_name>       
                       iam_role_crn: crn:v1:bluemix:public:iam::::role:Administrator      
                       iam_serviceid_crn: crn:v1:bluemix:public:iam-identity::<ID_string>       
        ```
        {: screen}
3.  Execute o script para configurar um projeto `ibm-observe` com uma conta de serviço privilegiado e um conjunto de daemon do Kubernetes para implementar o agente Sysdig em cada nó do trabalhador de seu cluster Kubernetes. O agente Sysdig coleta métricas, como o uso de CPU do nó do trabalhador, o uso de memória do nó do trabalhador, o tráfego HTTP para e de seus contêineres e os dados sobre vários componentes de infraestrutura. 

    No comando a seguir, substitua <sysdig_access_key> e <sysdig_collector_endpoint> pelos valores da chave de serviço que você criou anteriormente. Para <tag>, é possível associar tags a seu agente Sysdig, como `role:service,location:us-south` para ajudar a identificar o ambiente do qual as métricas são provenientes.

    ```
    curl -sL https://ibm.biz/install-sysdig-k8s-agent | bash -s -- -a <sysdig_access_key> -c <sysdig_collector_endpoint> -t <tag> -ac 'sysdig_capture_enabled: false' --openshift
    ```
    {: pre}
    
    Saída de exemplo: 
    ```
    * Detectando o sistema operacional
    * Fazendo download do yaml da função de cluster do Sysdig
    * Fazendo download do yaml do mapa de configuração do Sysdig
    * Fazendo download do yaml do daemonset v2 do Sysdig
    * Criando o projeto: ibm-observe
    * Criando o sysdig-agent serviceaccount no projeto: ibm-observe
    * Criando políticas de acesso do sysdig-agent
    * Criando o segredo do sysdig-agent usando o ACCESS_KEY fornecido
    * Recuperando o ID do cluster do IKS e o nome do cluster
    * Configurando o nome do cluster como <cluster_name>
    * Configurando ibm.containers-kubernetes.cluster.id 1fbd0c2ab7dd4c9bb1f2c2f7b36f5c47
    * Atualizando o configmap do agente e aplicando ao cluster
    * Configurando tags
    * Configurando o terminal do coletor
    * Incluindo configuração adicional no dragent.yaml
    * Ativando o
    configmap/sysdig-agent do Prometheus criado
    * Implementando o
    daemonset.extensions/sysdig-agent do agente sysdig criado
    ```
    {: screen}
        
4.  Verifique se os pods `sydig-agent` em cada nó mostram que os pods **1/1** estão prontos e se cada pod tem um status **Em execução**.
    ```
    oc get pods
    ```
    {: pre}
    
    Saída de exemplo:
    ```
    NAME                 READY     STATUS    RESTARTS   AGE
    sysdig-agent-qrbcq   1/1       Running   0          1m
    sysdig-agent-rhrgz   1/1       Running   0          1m
    ```
    {: screen}
5.  Em [Observabilidade do {{site.data.keyword.Bluemix_notm}} > Console de monitoramento](https://cloud.ibm.com/observe/logging), na linha para sua instância do {{site.data.keyword.mon_short}}, clique em **Visualizar Sysdig**. O painel Sysdig é aberto e é possível começar a analisar suas métricas do cluster.

Para obter mais informações sobre como usar o {{site.data.keyword.mon_short}}, consulte os [docs de Próximas etapas](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-kubernetes_cluster#kubernetes_cluster_next_steps).

### Opcional: limpando
{: #openshift_logdna_sysdig_cleanup}

Remova as instâncias do {{site.data.keyword.la_short}} e do {{site.data.keyword.mon_short}} de seu cluster e da conta do {{site.data.keyword.Bluemix_notm}}. Observe que, a menos que você armazene os logs e as métricas no [armazenamento persistente](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-archiving), não é possível acessar essas informações depois de excluir as instâncias de sua conta.
{: shortdesc}

1.  Limpe as instâncias do {{site.data.keyword.la_short}} e do {{site.data.keyword.mon_short}} em seu cluster removendo os projetos que você criou para eles. Quando você exclui um projeto, seus recursos, como contas de serviço e conjuntos de daemon, também são removidos.
    ```
    oc delete project logdna
    ```
    {: pre}
    ```
    oc delete project ibm-observe
    ```
    {: pre}
2.  Remova as instâncias de sua conta do {{site.data.keyword.Bluemix_notm}}.
    *   [Removendo uma instância do {{site.data.keyword.la_short}}](/docs/services/Log-Analysis-with-LogDNA?topic=LogDNA-remove).
    *   [Removendo uma instância do {{site.data.keyword.mon_short}}](/docs/services/Monitoring-with-Sysdig?topic=Sysdig-remove).

<br />


## Limitações
{: #openshift_limitations}

O beta do Red Hat OpenShift on IBM Cloud é liberado com as limitações a seguir.
{: shortdesc}

**Cluster**:
*   É possível criar somente clusters padrão, não clusters grátis.
*   Os locais estão disponíveis em duas áreas metropolitanas de multizona, Washington, DC e Londres. As zonas suportadas são `wdc04, wdc06, wdc07, lon04, lon05` e `lon06`.
*   Não é possível criar um cluster com nós do trabalhador que executam múltiplos sistemas operacionais, tais como o OpenShift no Red Hat Enterprise Linux e o Kubernetes nativo no Ubuntu.
*   O [ajustador automático de escala do cluster](/docs/containers?topic=containers-ca) não é suportado porque ele requer o Kubernetes versão 1.12 ou mais recente. O OpenShift 3.11 inclui somente o Kubernetes versão 1.11.



**Armazenamento**:
*   O armazenamento de objeto de arquivo, de bloco e de nuvem do {{site.data.keyword.Bluemix_notm}} é suportado. O software-defined storage (SDS) do Portworx não é suportado.
*   Devido à maneira como o armazenamento de arquivo NFS do {{site.data.keyword.Bluemix_notm}} configura as permissões de usuário do Linux, você pode encontrar erros ao usar o armazenamento de arquivo. Nesse caso, pode ser necessário configurar as [Restrições de contexto de segurança do OpenShift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html) ou usar um tipo de armazenamento diferente.

**Rede**:
*   O Calico é usado como o provedor de política de rede em vez do OpenShift SDN.

**Complementos, integrações e outros serviços**:
*   Os complementos do {{site.data.keyword.containerlong_notm}}, como o Istio, o Knative e o terminal do Kubernetes, não estão disponíveis.
*   Os gráficos do Helm não são certificados para funcionar em clusters do OpenShift, exceto no {{site.data.keyword.Bluemix_notm}} Object Storage.
*   Os clusters não são implementados com os segredos de pull de imagem para os domínios `icr.io` do {{site.data.keyword.registryshort_notm}}. É possível [criar seus próprios segredos de pull de imagem](/docs/containers?topic=containers-images#other_registry_accounts) ou, em vez disso, usar o registro do Docker integrado para clusters do OpenShift.

**Apps**:
*   O OpenShift define configurações de segurança mais estritas por padrão do que o Kubernetes nativo. Para obter mais informações, consulte os documentos do OpenShift de [Gerenciando as restrições de contexto de segurança (SCC) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.openshift.com/container-platform/3.11/admin_guide/manage_scc.html).
*   Por exemplo, os apps que são configurados para serem executados como raiz podem falhar, com os pods em um status `CrashLoopBackOff`. Para resolver esse problema, é possível modificar as restrições de contexto de segurança padrão ou usar uma imagem que não seja executada como raiz.
*   O OpenShift é configurado, por padrão, com um registro do Docker local. Se você deseja usar imagens que estão armazenadas em seus nomes de domínio `icr.io` do {{site.data.keyword.registrylong_notm}} privado remoto, deve-se criar os segredos para cada registro global e regional. É possível usar [copiar os segredos `default-<region>-icr-io`](/docs/containers?topic=containers-images#copy_imagePullSecret) do namespace `default` para o namespace do qual você deseja fazer pull de imagens ou [criar seu próprio segredo](/docs/containers?topic=containers-images#other_registry_accounts). Em seguida, [inclua o segredo de pull de imagem](/docs/containers?topic=containers-images#use_imagePullSecret) em sua configuração de implementação ou na conta de serviço do namespace.
*   O console do OpenShift é usado no lugar do painel do Kubernetes.

<br />


## O que Vem a Seguir?
{: #openshift_next}

Para obter mais informações sobre como trabalhar com seus apps e serviços de roteamento, consulte o [Developer Guide do OpenShift](https://docs.openshift.com/container-platform/3.11/dev_guide/index.html).

<br />


## Feedback e perguntas
{: #openshift_support}

Durante o beta, os clusters do Red Hat OpenShift on IBM Cloud não são cobertos pelo Suporte IBM nem pelo Suporte do Red Hat. Qualquer suporte fornecido é para ajudar a avaliar o produto em preparação para sua disponibilidade geral.
{: important}

Para qualquer pergunta ou feedback, poste no Slack. 
*   Se você for um usuário externo, poste no canal [#openshift ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com/messages/CKCJLJCH4). 
*   Se você for um IBMista, use o canal [#iks-openshift-users ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-argonauts.slack.com/messages/CJH0UPN2D).

Se você não usa um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para esse Slack.
{: tip}
