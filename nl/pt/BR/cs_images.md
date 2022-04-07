---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-04"

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



# Construindo contêineres de imagens
{: #images}

Uma imagem do Docker é a base para cada contêiner que você cria com o {{site.data.keyword.containerlong}}.
{:shortdesc}

Uma imagem é criada por meio de um Dockerfile, que é um arquivo que contém instruções para construir a imagem. Um Dockerfile pode
referenciar os artefatos de construção em suas instruções que são armazenadas separadamente, como um app, a configuração
do app e suas dependências.

## Planejando registros de imagem
{: #planning_images}

As imagens geralmente são armazenadas em um registro que pode ser acessado pelo público (registro público) ou configurado com acesso
limitado para um pequeno grupo de usuários (registro privado).
{:shortdesc}

Os registros
públicos, como Docker Hub, podem ser usados na introdução ao Docker e Kubernetes para criar seu
primeiro app conteinerizado em um cluster. Mas quando se trata de aplicativos corporativos, use um registro
privado como aquele fornecido no {{site.data.keyword.registryshort_notm}} para proteger suas imagens de serem usadas
e mudadas por usuários não autorizados. Os registros
privados devem ser configurados pelo administrador de cluster para assegurar que as credenciais para acessar o registro
privado estejam disponíveis para os usuários do cluster.


É possível usar múltiplos registros com o {{site.data.keyword.containerlong_notm}} para implementar apps em seu cluster.

|Registro|Descrição|Benefício|
|--------|-----------|-------|
|[{{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#getting-started)|Com essa opção, é possível configurar o seu próprio repositório de imagem do Docker seguro no {{site.data.keyword.registryshort_notm}} no qual é possível armazenar e compartilhar as imagens com segurança entre
usuários do cluster.|<ul><li>Gerencie o acesso a imagens em sua conta.</li><li>Use imagens e apps de amostra fornecidos pela {{site.data.keyword.IBM_notm}}, como o {{site.data.keyword.IBM_notm}} Liberty, como uma imagem pai e inclua seu próprio código de app nela.</li><li>Varredura automática de imagens para potenciais vulnerabilidades pelo Vulnerability Advisor, incluindo
recomendações específicas do S.O. para corrigi-las.</li></ul>|
|Qualquer outro registro privado|Conecte qualquer registro privado existente ao seu cluster criando um [segredo de extração de imagem ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/containers/images/). O segredo é usado para salvar com segurança sua URL de registro e credenciais em um
segredo do Kubernetes.|<ul><li>Use os registros privados existentes independentemente de sua origem (Docker Hub, registros pertencentes
à organização ou outros registros de Nuvem privada).</li></ul>|
|[Docker Hub público![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://hub.docker.com/){: #dockerhub}|Use essa opção para usar imagens públicas existentes do Docker Hub diretamente em sua [implementação do Kubernetes![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/), quando nenhuma mudança do Dockerfile for necessária. <p>**Nota:** lembre-se de que essa opção poderá não atender aos requisitos de segurança de sua organização, como gerenciamento de acesso, varredura de vulnerabilidade ou privacidade de app.</p>|<ul><li>Nenhuma configuração adicional é necessária para o seu cluster.</li><li>Inclui uma variedade de aplicativos de software livre.</li></ul>|
{: caption="Opções de registro de imagem pública e privada" caption-side="top"}

Depois de configurar um registro de imagens, os usuários do cluster poderão usá-las para implementar aplicativos no cluster.

Saiba mais sobre [como proteger suas informações pessoais](/docs/containers?topic=containers-security#pi) quando trabalhar com imagens de contêiner.

<br />


## Configurando o conteúdo confiável para imagens de contêiner
{: #trusted_images}

É possível construir contêineres de imagens confiáveis assinadas e armazenadas no {{site.data.keyword.registryshort_notm}} e evitar as implementações de imagens não assinadas ou vulneráveis.
{:shortdesc}

1.  [Conectar imagens para o conteúdo confiável](/docs/services/Registry?topic=registry-registry_trustedcontent#registry_trustedcontent). Depois de configurar a confiança para suas imagens, será possível gerenciar o conteúdo confiável e os assinantes que podem enviar imagens por push para seu registro.
2.  Para aplicar uma política na qual apenas imagens assinadas podem ser usadas para construir contêineres em seu cluster, [inclua o Container Image Security Enforcement (beta)](/docs/services/Registry?topic=registry-security_enforce#security_enforce).
3.  Implemente seu app.
    1. [Implemente no namespace `default` do Kubernetes](#namespace).
    2. [Implemente em um namespace diferente do Kubernetes ou em uma região ou uma conta diferente do {{site.data.keyword.Bluemix_notm}}](#other).

<br />


## Implementando contêineres por meio de uma imagem do {{site.data.keyword.registryshort_notm}} no namespace `default` do Kubernetes
{: #namespace}

É possível implementar contêineres em seu cluster por meio de uma imagem pública fornecida pela IBM ou de uma imagem privada que é armazenada em seu namespace do {{site.data.keyword.registryshort_notm}}. Para obter mais informações sobre como seu cluster acessa imagens de registro, consulte [Compreendendo como seu cluster está autorizado a fazer pull de imagens do {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth).
{:shortdesc}

Antes de iniciar:
1. [Configure um namespace no {{site.data.keyword.registryshort_notm}} e envie por push as imagens para esse namespace](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Criar um cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3. Se tiver um cluster existente criado antes de **25 de fevereiro de 2019**, [atualize-o para usar a chave de API `imagePullSecret`](#imagePullSecret_migrate_api_key).
4. [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para implementar um contêiner no namespace **padrão** de seu cluster:

1.  Crie um arquivo de configuração de implementação que é chamado de `mydeployment.yaml`.
2.  Defina a implementação e a imagem a serem usadas de seu namespace no {{site.data.keyword.registryshort_notm}}.

    ```
    apiVersion: apps/v1 kind: Deployment metadata: name: <app_name>-deployment spec: replicas: 3 selector: matchLabels: app: <app_name> template: metadata: labels: app: <app_name> spec: containers:
          - name: <app_name>
            image: <region>.icr.io/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    Substitua as variáveis da URL da imagem pelas informações para sua imagem:
    *  **`<app_name>`**: o nome de seu aplicativo.
    *  **`<region>`**: o terminal de API regional do {{site.data.keyword.registryshort_notm}} para o domínio do registro. Para listar o domínio para a região na qual você está com login efetuado, execute `ibmcloud cr api`.
    *  **`<namespace>`**: o namespace do registro. Para obter suas informações de namespace, execute `ibmcloud cr namespace-list`.
    *  **`<my_image>:<tag>`**: a imagem e a tag que serão usadas para construir o contêiner. Para obter as imagens disponíveis em seu registro, execute `ibmcloud cr images`.

3.  Crie a implementação em seu cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

<br />


## Entendendo como autorizar seu cluster a fazer pull de imagens de um registro
{: #cluster_registry_auth}

Para fazer pull de imagens de um registro, seu cluster do {{site.data.keyword.containerlong_notm}} usa um tipo especial de segredo do Kubernetes, um `imagePullSecret`. Esse segredo de pull de imagem armazena as credenciais para acessar um registro de contêiner. O registro do contêiner pode ser seu namespace no {{site.data.keyword.registrylong_notm}}, um namespace no {{site.data.keyword.registrylong_notm}} que pertence a uma conta do {{site.data.keyword.Bluemix_notm}} diferente ou qualquer outro registro privado, como o Docker. Seu cluster é configurado para fazer pull de imagens de seu namespace no {{site.data.keyword.registrylong_notm}} e implementar contêineres dessas imagens no namespace `default` do Kubernetes em seu cluster. Se precisar fazer pull de imagens em outros namespaces do cluster Kubernetes ou outros registros, você deverá configurar o segredo de pull de imagem.
{:shortdesc}

**Como meu cluster é configurado para fazer pull de imagens do namespace `default` do Kubernetes?**<br>
Ao criar um cluster, o cluster tem um ID de serviço do {{site.data.keyword.Bluemix_notm}} IAM que é fornecido uma política de função de acesso de serviço do IAM **Leitor** para {{site.data.keyword.registrylong_notm}}. As credenciais do ID de serviço são personificadas em uma chave de API não expirada que é armazenada em segredos de extração de imagem em seu cluster. Os segredos de extração de imagem são incluídos no namespace `default` do Kubernetes e na lista de segredos na conta de serviço `default` para esse namespace. Usando segredos de extração de imagem, suas implementações podem extrair imagens (acesso somente leitura) em seu [registro global e regional](/docs/services/Registry?topic=registry-registry_overview#registry_regions) para construir contêineres no namespace `default` do Kubernetes. O registro global armazena com segurança as imagens públicas fornecidas pela IBM às quais é possível se referir em suas implementações em vez de ter referências diferentes para imagens que são armazenadas em cada registro regional. O registro regional armazena com segurança suas próprias imagens privadas do Docker.

**Posso restringir o acesso de pull para um determinado registro regional?**<br>
Sim, é possível [editar a política IAM existente do ID de serviço](/docs/iam?topic=iam-serviceidpolicy#access_edit) que restringe a função de acesso ao serviço **Leitor** a esse registro regional ou a um recurso de registro, como um namespace. Antes de poder customizar as políticas de registro do IAM, deve-se [ativar as políticas do {{site.data.keyword.Bluemix_notm}} IAM para o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-user#existing_users).

  Deseja tornar as credenciais de registro ainda mais seguras? Peça ao administrador de cluster para [ativar o {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) em seu cluster para criptografar segredos do Kubernetes em seu cluster, como o `imagePullSecret` que armazena suas credenciais de registro.
  {: tip}

**Posso fazer pull de imagens em namespaces do Kubernetes diferentes do `default`?**<br>
Não por padrão. Usando a configuração do cluster padrão, é possível implementar contêineres de qualquer imagem que esteja armazenada em seu namespace do {{site.data.keyword.registrylong_notm}} no namespace `default` do Kubernetes de seu cluster. Para usar essas imagens em outros namespaces do Kubernetes ou em outras contas do {{site.data.keyword.Bluemix_notm}}, [é possível optar por copiar ou criar seu próprio segredo de pull de imagem](#other).

**Posso fazer pull de imagens de uma conta diferente do {{site.data.keyword.Bluemix_notm}}?**<br>
Sim, crie uma chave de API na conta do {{site.data.keyword.Bluemix_notm}} que deseja usar. Em seguida, crie um segredo de pull de imagem que armazene essas credenciais de chave de API em cada cluster e namespace de cluster do qual deseja fazer pull. [Acompanhe esse exemplo que usa uma chave de API de um ID de serviço autorizado](#other_registry_accounts).

Para usar um registro que não é do {{site.data.keyword.Bluemix_notm}}, como o Docker, consulte [Acessando imagens armazenadas em outros registros privados](#private_images).

**A chave de API precisa ser destinada a um ID de serviço? O que acontecerá se eu atingir o limite de IDs de serviço para minha conta?**<br>
A configuração de cluster padrão cria um ID de serviço para armazenar as credenciais de chave de API do {{site.data.keyword.Bluemix_notm}} IAM no segredo de pull de imagem. No entanto, também é possível criar uma chave de API para um usuário individual e armazenar essas credenciais em um segredo de pull de imagem. Se você atingir o limite de [IDs de serviço do IAM](/docs/iam?topic=iam-iam_limits#iam_limits), seu cluster será criado sem o ID de serviço e o segredo de pull de imagem e não poderá fazer pull de imagens dos domínios do registro `icr.io` por padrão. Você deverá [criar seu próprio segredo de pull de imagem](#other_registry_accounts), mas usando uma chave de API para um usuário individual, como um ID funcional, e não um ID de serviço do {{site.data.keyword.Bluemix_notm}} IAM.

**Meu segredo de pull de imagem de cluster usa um token de registro. Um token ainda funciona?**<br>

O método anterior de autorizar o acesso do cluster ao {{site.data.keyword.registrylong_notm}} criando automaticamente um [token](/docs/services/Registry?topic=registry-registry_access#registry_tokens) e armazenando-o em um segredo de extração de imagem é suportado, mas descontinuado.
{: deprecated}

Os tokens autorizam o acesso aos domínios de registro `registry.bluemix.net` descontinuados, enquanto as chaves de API autorizam o acesso aos domínios de registro `icr.io`. Durante o período de transição da autenticação baseada em token para a baseada em API, tanto os tokens quanto os segredos de pull de imagem baseados em chave de API são criados por um tempo. Com token e segredos de pull de imagem baseados em chave de API, seu cluster pode fazer pull de imagens de domínios `registry.bluemix.net` ou `icr.io` no namespace `default` do Kubernetes.

Antes que os tokens descontinuados e os domínios `registry.bluemix.net` se tornem não suportados, atualize seus segredos de pull de imagem de cluster para usar o método de chave de API para o [namespace `default` do Kubernetes](#imagePullSecret_migrate_api_key) e [quaisquer outros namespaces ou contas](#other) que possam ser usados. Em seguida, atualize suas implementações para fazer pull dos domínios de registro `icr.io`.

**Depois de copiar ou criar um segredo de pull de imagem em outro namespace do Kubernetes, estou pronto?**<br>
Não totalmente. Seus contêineres devem estar autorizados a fazer pull de imagens usando o segredo que você criou. É possível incluir o segredo de extração de imagem na conta de serviço do namespace ou referir-se ao segredo em cada implementação. Para obter instruções, consulte [Usando o segredo de extração de imagem para implementar contêineres](/docs/containers?topic=containers-images#use_imagePullSecret).

<br />


## Atualizando os Clusters Existentes para Utilizar a Imagem de Chave da API Pull Segredo
{: #imagePullSecret_migrate_api_key}

Novos clusters do {{site.data.keyword.containerlong_notm}} armazenam uma chave de API em [um segredo de pull de imagem para autorizar o acesso ao {{site.data.keyword.registrylong_notm}}](#cluster_registry_auth). Com esses segredos de extração de imagem, é possível implementar contêineres de imagens que são armazenados nos domínios de registro `icr.io`. Para clusters que foram criados antes de **25 de fevereiro de 2019**, deve-se atualizar o cluster para armazenar uma chave de API em vez de um token de registro no segredo de extração de imagem.
{: shortdesc}

** Antes de iniciar **:
*   [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*   Certifique-se de que você tenha as permissões a seguir:
    *   Função da plataforma **Operador ou Administrador** do {{site.data.keyword.Bluemix_notm}} IAM para o {{site.data.keyword.containerlong_notm}}. O proprietário da conta pode conceder a você a função executando:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name containers-kubernetes --roles Administrator,Operator
        ```
        {: pre}
    *   Função da plataforma **Administrador** do {{site.data.keyword.Bluemix_notm}} IAM para o {{site.data.keyword.registrylong_notm}}, em todas as regiões e grupos de recursos. O proprietário da conta pode conceder a você a função executando:
        ```
        ibmcloud iam user-policy-create <your_user_email> --service-name container-registry --roles Administrator
        ```
        {: pre}

**Para atualizar seu segredo de pull de imagem de cluster no namespace `default` do Kubernetes**:
1.  Obtenha seu ID do cluster.
    ```
    ibmcloud ks clusters
    ```
    {: pre}
2.  Execute o comando a seguir para criar um ID de serviço para o cluster, designe ao ID de serviço uma função de serviço **Leitor** do IAM para o {{site.data.keyword.registrylong_notm}}, crie uma chave de API para personificar as credenciais do ID de serviço e armazene a chave de API em um segredo de extração de imagem do Kubernetes no cluster. O segredo de extração de imagem está no namespace `default` do Kubernetes.
    ```
    ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Quando você executa esse comando, a criação de credenciais do IAM e de segredos de extração de imagem é iniciada e pode levar algum tempo para ser concluída. Não será possível implementar contêineres que puxam uma imagem dos domínios `icr.io` do {{site.data.keyword.registrylong_notm}} até que os segredos de extração de imagem sejam criados.
    {: important}

3.  Verifique se os segredos de extração de imagem são criados em seu cluster. Observe que você tem um segredo de extração de imagem separado para cada região do {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets
    ```
    {: pre}
    Saída de exemplo:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
4.  Atualize suas implementações de contêiner para extrair imagens do nome de domínio `icr.io`.
5.  Opcional: se você tiver um firewall, certifique-se de que o [permita o tráfego de rede de saída para as sub-redes de registro](/docs/containers?topic=containers-firewall#firewall_outbound) para os domínios que você usa.

**O que vem a seguir?**
*   Para extrair imagens em namespaces do Kubernetes diferentes de `default` ou de outras contas do {{site.data.keyword.Bluemix_notm}}, [copie ou crie outro segredo de extração de imagem](/docs/containers?topic=containers-images#other).
*   Para restringir o acesso ao segredo de extração de imagem a recursos de registro específicos, como namespaces ou regiões:
    1.  Certifique-se de que [{{site.data.keyword.Bluemix_notm}} políticas do IAM para {{site.data.keyword.registrylong_notm}} estão ativadas](/docs/services/Registry?topic=registry-user#existing_users).
    2.  [Edite as {{site.data.keyword.Bluemix_notm}} políticas do IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit) para o ID de serviço ou [crie outra imagem de extração de imagem](/docs/containers?topic=containers-images#other_registry_accounts).

<br />


## Usando um segredo de pull de imagem para acessar outros namespaces do cluster Kubernetes, outras contas do {{site.data.keyword.Bluemix_notm}} ou registros privados externos
{: #other}

Configure seu próprio segredo de pull de imagem em seu cluster para implementar contêineres em namespaces do Kubernetes diferentes do `default` e use imagens armazenadas em outras contas do {{site.data.keyword.Bluemix_notm}} ou em registros privados externos. Além disso, você pode criar seu próprio segredo de pull de imagem para aplicar políticas de acesso do IAM que restringem permissões a namespaces ou ações de imagem de registro específicos (como `push` ou `pull`).
{:shortdesc}

Depois de criar o segredo de pull de imagem, seus contêineres devem usar o segredo para serem autorizados a fazer pull de uma imagem do registro. É possível incluir o segredo de extração de imagem na conta de serviço do namespace ou referir-se ao segredo em cada implementação. Para obter instruções, consulte [Usando o segredo de extração de imagem para implementar contêineres](/docs/containers?topic=containers-images#use_imagePullSecret).

Os segredos de extração de imagem são válidos apenas para os namespaces do Kubernetes para os qual eles foram criados. Repita essas etapas para cada namespace no qual você desejar implementar contêineres. Imagens do [DockerHub](#dockerhub) não requerem segredos de extração de imagem.
{: tip}

Antes de iniciar:

1.  [Configure um namespace no {{site.data.keyword.registryshort_notm}} e envie por push as imagens para esse namespace](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2.  [Criar um cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3.  Se você tiver um cluster existente criado antes de **25 de fevereiro de 2019**, [atualize-o para usar o segredo de pull de imagem da chave de API](#imagePullSecret_migrate_api_key).
4.  [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

<br/>
Para usar seu próprio segredo de extração de imagem, escolha entre as opções a seguir:
- [Copie o segredo de pull de imagem](#copy_imagePullSecret) do namespace padrão do Kubernetes para outros namespaces em seu cluster.
- [Crie novas credenciais de chave de API do IAM e as armazene em um segredo de pull de imagem](#other_registry_accounts) para acessar imagens em outras contas do {{site.data.keyword.Bluemix_notm}} ou para aplicar políticas IAM que restrinjam o acesso a determinados domínios de registro ou namespaces.
- [Crie um segredo de extração de imagem para acessar imagens em registros privados externos](#private_images).

<br/>
Se você já tiver criado um segredo de extração de imagem em seu namespace que você deseja usar em sua implementação, consulte [Implementando contêineres usando o `imagePullSecret` criado](#use_imagePullSecret).

### Copiando um segredo de pull de imagem existente
{: #copy_imagePullSecret}

É possível copiar um segredo de pull de imagem, como aquele criado automaticamente para o namespace `default` do Kubernetes, para outros namespaces em seu cluster. Em vez disso, se desejar usar credenciais de chave de API do {{site.data.keyword.Bluemix_notm}} IAM diferentes para esse namespace, como a restrição do acesso a namespaces específicos ou o pull de imagens de outras contas do {{site.data.keyword.Bluemix_notm}}, [crie um segredo de pull de imagem](#other_registry_accounts).
{: shortdesc}

1.  Liste os namespaces do Kubernetes disponíveis em seu cluster ou crie um namespace para usar.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Saída de exemplo:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Para criar um namespace:
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Liste os segredos de extração de imagem existentes no namespace `default` do Kubernetes para o {{site.data.keyword.registrylong_notm}}.
    ```
    kubectl get secrets -n default | grep icr
    ```
    {: pre}
    Saída de exemplo:
    ```
    default-us-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-uk-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-de-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-au-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-jp-icr-io                          kubernetes.io/dockerconfigjson        1         16d default-icr-io                             kubernetes.io/dockerconfigjson        1         16d
    ```
    {: screen}
3.  Copie cada segredo de extração de imagem do namespace `default` para o namespace de sua escolha. Os novos segredos de pull de imagem são nomeados `<namespace_name>-icr-<region>-io`.
    ```
    kubectl get secret default-us-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-uk-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-de-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-au-icr-io -o yaml | sed 's/default/ < new-namespace>/g ' | kubectl -n < new-namespace >create -f-f
    ```
    {: pre}
    ```
    kubectl get secret default-jp-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
    ```
    kubectl get secret default-icr-io -o yaml | sed 's/default/<new-namespace>/g' | kubectl -n <new-namespace> create -f -
    ```
    {: pre}
4.  Verifique se os segredos foram criados com êxito.
    ```
    kubectl get secrets -n <namespace_name>
    ```
    {: pre}
5.  [Inclua o segredo de extração de imagem em uma conta de serviço do Kubernetes para que qualquer pod no namespace possa usar o segredo de extração de imagem ao implementar um contêiner](#use_imagePullSecret).

### Criando um segredo de pull de imagem com credenciais de chave de API do IAM diferentes para obter mais controle ou acesso a imagens em outras contas do {{site.data.keyword.Bluemix_notm}}
{: #other_registry_accounts}

É possível designar as políticas de acesso do {{site.data.keyword.Bluemix_notm}} IAM aos usuários ou um ID de serviço para restringir permissões a namespaces ou ações de imagem de registro específicos (como `push` ou `pull`). Em seguida, crie uma chave de API e armazene essas credenciais de registro em um segredo de pull de imagem para seu cluster.
{: shortdesc}

Por exemplo, para acessar imagens em outras contas do {{site.data.keyword.Bluemix_notm}}, crie uma chave de API que armazene as credenciais do {{site.data.keyword.registryshort_notm}} de um ID de serviço ou usuário nessa conta. Em seguida, na conta de seu cluster, salve as credenciais da chave de API em um segredo de pull de imagem para cada cluster e namespace de cluster.

As etapas a seguir criam uma chave de API que armazena as credenciais de um ID de serviço do {{site.data.keyword.Bluemix_notm}} IAM. Em vez de usar um ID de serviço, você pode desejar criar uma chave de API para um ID do usuário que tenha uma política de acesso de serviço do {{site.data.keyword.Bluemix_notm}} IAM para o {{site.data.keyword.registryshort_notm}}. No entanto, certifique-se de que o usuário seja um ID funcional ou tenha um plano no caso de o usuário sair para que o cluster possa ainda acessar o registro.
{: note}

1.  Liste os espaços de nomes Kubernetes disponíveis em seu cluster ou crie um namespace para usar onde você deseja implementar contêineres das imagens de registro.
    ```
    kubectl get namespaces
    ```
    {: pre}

    Saída de exemplo:
    ```
    default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
    ```
    {: screen}

    Para criar um namespace:
    ```
    kubectl create namespace <namespace_name>
    ```
    {: pre}
2.  Crie um ID de serviço do {{site.data.keyword.Bluemix_notm}} IAM para seu cluster que é usado para as políticas do IAM e as credenciais da chave de API no segredo de extração de imagem. Certifique-se de dar ao ID do serviço uma descrição que ajude a recuperar o ID de serviço mais tarde, como incluir o nome do cluster e do namespace.
    ```
    ibmcloud iam service-id-create <cluster_name>-<kube_namespace>-id --description "Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
3.  Crie uma política do {{site.data.keyword.Bluemix_notm}} IAM customizada para seu ID de serviço de cluster que conceda acesso ao {{site.data.keyword.registryshort_notm}}.
    ```
    ibmcloud iam service-policy-create <cluster_service_ID> --roles <service_access_role> --service-name container-registry [--region <IAM_region>] [--resource-type namespace --resource <registry_namespace>]
    ```
    {: pre}
    <table>
    <caption>Entendendo os componentes deste comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code> <em> &lt;cluster_service_ID&gt; </em> </code></td>
    <td>Necessário. Substitua pelo ID de serviço `<cluster_name>-<kube_namespace>-id` criado anteriormente para seu cluster Kubernetes.</td>
    </tr>
    <tr>
    <td><code> -- service-name  <em> container-registry </em> </code></td>
    <td>Necessário. Insira `container-registry` para que a política do IAM seja para o {{site.data.keyword.registrylong_notm}}.</td>
    </tr>
    <tr>
    <td><code>-- funções <em>&lt;service_access_role&gt;</em></code></td>
    <td>Necessário. Insira a [função de acesso de serviço para o {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-iam#service_access_roles) para o qual você deseja definir o escopo para o acesso de ID de serviço. Os valores possíveis são `Reader`, `Writer` e `Manager`.</td>
    </tr>
    <tr>
    <td><code> -- região  <em> &lt;IAM_region&gt; </em> </code></td>
    <td>Opcional. Se você desejar definir o escopo da política de acesso para determinadas regiões do IAM, insira as regiões em uma lista separada por vírgula. Os valores possíveis são `au-syd`, `eu-gb`, `eu-de`, `jp-tok`, `us-south` e `global`.</td>
    </tr>
    <tr>
    <td><code> -- resource-type  <em> namespace </em>  -- resource  <em> &lt;registry_namespace&gt; </em> </code></td>
    <td>Opcional. Se desejar limitar o acesso a apenas imagens em determinados [namespaces do {{site.data.keyword.registrylong_notm}}](/docs/services/Registry?topic=registry-registry_overview#registry_namespaces), insira `namespace` para o tipo de recurso e especifique o `<registry_namespace>`. Para listar namespaces de registro, execute `ibmcloud cr namespaces`.</td>
    </tr>
    </tbody></table>
4.  Crie uma chave de API para o ID de serviço. Nomeie a chave de API de forma semelhante ao seu ID de serviço e inclua o ID de serviço criado anteriormente, ``<cluster_name>-<kube_namespace>-id`. Certifique-se de dar à chave de API uma descrição que ajuda a recuperar a chave posteriormente.
    ```
    ibmcloud iam service-api-key-create <cluster_name>-<kube_namespace>-key <cluster_name>-<kube_namespace>-id --description "API key for service ID <service_id> in Kubernetes cluster <cluster_name> namespace <kube_namespace>"
    ```
    {: pre}
5.  Recupere seu valor de **Chave de API** da saída do comando anterior.
    ```
    Preservar a chave de API! Não é possível recuperá-la após sua criação.

    Nome < cluster_name>-< kube_namespace>-key   
    Descrição key_for_registry_for_serviceid_for_kubernetes_cluster_multizone_namespace_test   
    Ligado a      crn:v1:bluemix:public:iam-identity::a/1bb222bb2b33333ddd3d3333ee4ee444::serviceid:ServiceId-ff55555f-5fff-6666-g6g6-777777h7h7hh   
    Criado em 2019-02-01T19:06 + 0000   
    Chave de API       i-8i88ii8jjjj9jjj99kkkkkkkkk_k9-llllll11mmm1   
    Bloqueado        false   
    UUID          ApiKey-222nn2n2-o3o3-3o3o-4p44-oo444o44o4o4   
    ```
    {: screen}
6.  Crie um segredo de extração de imagem do Kubernetes para armazenar as credenciais da chave de API no namespace do cluster. Repita esta etapa para cada domínio `icr.io`, namespace do Kubernetes e cluster que você deseja extrair imagens do registro com as credenciais do IAM desse ID de serviço.
    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name> --docker-server=<registry_URL> --docker-username=iamapikey --docker-password=<api_key_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Entendendo os componentes deste comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Necessário. Especifique o namespace do Kubernetes de seu cluster que você usou para o nome do ID do serviço.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Necessário. Insira um nome para seu segredo de extração de imagem.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Necessário. Configure a URL para o registro de imagem no qual o namespace de registro está configurado. Domínios de registro disponíveis:<ul>
    <li>AP Norte (Tóquio):  ` jp.icr.io `</li>
    <li>AP Sul (Sydney):  ` au.icr.io `</li>
    <li>Central da UE (Frankfurt):  ` de.icr.io `</li>
    <li>Sul do Reino Unido (Londres):  ` uk.icr.io `</li>
    <li>Sul dos EUA (Dallas):  ` us.icr.io `</li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username iamapikey</code></td>
    <td>Necessário. Insira o nome do usuário para efetuar login em seu registro privado. Para o {{site.data.keyword.registryshort_notm}}, o nome do usuário é configurado como o valor <strong><code>iamapikey</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Necessário. Insira o valor de sua **Chave de API** que você recuperou anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Necessário. Se você tiver um, insira seu endereço de e-mail do Docker. Se você não fizer isso, insira um endereço de e-mail fictício, como `a@b.c`. Esse e-mail é necessário para criar um segredo do Kubernetes, mas não é usado após a criação.</td>
    </tr>
    </tbody></table>
7.  Verifique se o segredo foi criado com êxito. Substitua <em>&lt;kubernetes_namespace&gt;</em> pelo namespace no qual você criou o segredo de extração de imagem.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}
8.  [Inclua o segredo de extração de imagem em uma conta de serviço do Kubernetes para que qualquer pod no namespace possa usar o segredo de extração de imagem ao implementar um contêiner](#use_imagePullSecret).

### Acessando imagens que são armazenadas em outros registros privados
{: #private_images}

Se você já tiver um registro privado, deverá armazenar as credenciais de registro em um segredo de imagem do Kubernetes e referenciar esse segredo por meio de seu arquivo de configuração.
{:shortdesc}

Antes de iniciar:

1.  [Criar um cluster](/docs/containers?topic=containers-clusters#clusters_ui).
2.  [Destine sua CLI para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Para criar um segredo de extração de imagem:

1.  Crie o segredo do Kubernetes para armazenar suas credenciais de registro privado.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=<docker_username> --docker-password=<docker_password> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Entendendo os componentes deste comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Necessário. O namespace do Kubernetes do cluster no qual você deseja usar o segredo e implementar contêineres. Execute <code>kubectl get namespaces</code> para listar todos os namespaces em seu cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Necessário. O nome que você deseja usar para seu <code>imagePullSecret</code>.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Necessário. A URL para o registro no qual as imagens privadas são armazenadas.</td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Necessário. O nome do usuário para efetuar login no seu registro privado.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Necessário. O valor do token de registro que você recuperou anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Necessário. Se você tiver um, insira seu endereço de e-mail do Docker. Se você não tiver um, insira um endereço de e-mail fictício, como `a@b.c`. Esse e-mail é necessário para criar um segredo do Kubernetes, mas não é usado após a criação.</td>
    </tr>
    </tbody></table>

2.  Verifique se o segredo foi criado com êxito. Substitua <em>&lt;kubernetes_namespace&gt;</em> pelo nome do namespace em que você criou o `imagePullSecret`.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

3.  [Crie um pod que faça referência ao segredo de extração de imagem](#use_imagePullSecret).

<br />


## Usando o segredo de pull de imagem para implementar contêineres
{: #use_imagePullSecret}

É possível definir um segredo de extração de imagem em sua implementação de pod ou armazenar o segredo de extração de imagem em sua conta de serviço do Kubernetes para que ela esteja disponível para todas as implementações que não especificam uma conta de serviço.
{: shortdesc}

Escolha entre as opções a seguir:
* [Referindo-se ao segredo de extração de imagem em sua implementação de pod](#pod_imagePullSecret): use essa opção se você não desejar conceder acesso ao seu registro para todos os pods em seu namespace por padrão.
* [Armazenando o segredo de pull de imagem na conta de serviço do Kubernetes](#store_imagePullSecret): use essa opção para conceder acesso a imagens em seu registro para todas as implementações nos namespaces selecionados do Kubernetes.

Antes de iniciar:
* [Crie um segredo de pull de imagem](#other) para acessar imagens em outros registros ou namespaces do Kubernetes diferentes do `default`.
* [Destine sua CLI para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

### Referindo-se à imagem pull secret em sua implementação pod
{: #pod_imagePullSecret}

Quando você se refere ao segredo de extração de imagem em uma implementação de pod, ele é válido somente para esse pod e não pode ser compartilhado entre os pods no namespace.
{:shortdesc}

1.  Crie um arquivo de configuração de pod que seja denominado `mypod.yaml`.
2.  Defina o pod e o segredo de pull de imagem para acessar imagens no {{site.data.keyword.registrylong_notm}}.

    Para acessar uma imagem privada:
    ```
    apiVersion: v1 kind: Pod metadata: name: mypod spec: containers:
        - name: <container_name>
          image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    Para acessar uma imagem pública do {{site.data.keyword.Bluemix_notm}}:
    ```
    apiVersion: v1 kind: Pod metadata: name: mypod spec: containers:
        - name: <container_name>
          image: icr.io/<image_name>:<tag>
      imagePullSecrets:
        - name: <secret_name>
    ```
    {: codeblock}

    <table>
    <caption>Entendendo os componentes de arquivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;container_name&gt;</em></code></td>
    <td>O nome do contêiner a ser implementado em seu cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;namespace_name&gt;</em></code></td>
    <td>O namespace no qual a imagem é armazenada. Para listar os namespaces disponíveis, execute `ibmcloud cr namespace-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;image_name&gt;</em></code></td>
    <td>O nome da imagem a ser usada. Para listar as imagens disponíveis em uma conta do {{site.data.keyword.Bluemix_notm}}, execute `ibmcloud cr image-list`.</td>
    </tr>
    <tr>
    <td><code><em>&lt;tag&gt;</em></code></td>
    <td>A versão da imagem que você deseja usar. Se nenhuma tag for especificada, a imagem identificada como <strong>mais recente</strong> será usada por padrão.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>O nome do segredo de extração de imagem que você criou anteriormente.</td>
    </tr>
    </tbody></table>

3.  Salve as suas mudanças.
4.  Crie a implementação em seu cluster.
    ```
    kubectl apply -f mypod.yaml
    ```
    {: pre}

### Armazenando o segredo de extração de imagem na conta de serviço do Kubernetes para o namespace selecionado
{:#store_imagePullSecret}

Cada namespace tem uma conta de serviço do Kubernetes denominada `default`. É possível incluir o segredo de extração de imagem nessa conta de serviço para conceder acesso a imagens em seu registro. As implementações que não especificam uma conta de serviço usarão automaticamente a conta de serviço `default` para este namespace.
{:shortdesc}

1. Verifique se um segredo de extração de imagem já existe para sua conta de serviço padrão.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}
   Quando `<none>` é exibido na entrada **Segredo de pull de imagem**, não existe nenhum segredo de pull de imagem.  
2. Inclua o segredo de extração de imagem em sua conta de serviço padrão.
   - **Para incluir o segredo de extração de imagem quando nenhum segredo de extração de imagem está definido:**
       ```
       kubectl patch -n <namespace_name> serviceaccount/default -p '{"imagePullSecrets":[{"name": "<image_pull_secret_name>"}]}'
       ```
       {: pre}
   - **Para incluir o segredo de extração de imagem quando um segredo de extração de imagem já está definido:**
       ```
       kubectl patch -n < namespace_name>serviceaccount/default --type = 'json' -p= '[ { { "op": "add", "path" :" /imagePullSecrets/-", "value": "{" name ":" < image_pull_secret_name>" } } ]'
       ```
       {: pre}
3. Verifique se o segredo de extração de imagem foi incluído em sua conta de serviço padrão.
   ```
   kubectl describe serviceaccount default -n <namespace_name>
   ```
   {: pre}

   Saída de exemplo:
   ```
   Name:                default
   Namespace:           <namespace_name>
   Labels:              <none>
   Annotations:         <none>
   Image pull secrets:  <image_pull_secret_name>
   Mountable secrets:   default-token-sh2dx
   Tokens:              default-token-sh2dx
   Events:              <none>
   ```
   {: pre}

4. Implemente um contêiner de uma imagem em seu registro.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: mypod
   spec:
     containers:
       - name: <container_name>
         image: <region>.icr.io/<namespace_name>/<image_name>:<tag>
   ```
   {: codeblock}

5. Crie a implementação no cluster.
   ```
   kubectl apply -f mypod.yaml
   ```
   {: pre}

<br />


## Descontinuado: usando um token de registro para implementar contêineres por meio de uma imagem do {{site.data.keyword.registrylong_notm}}
{: #namespace_token}

É possível implementar contêineres em seu cluster de uma imagem pública fornecida pela IBM ou de uma imagem privada que é armazenada em seu namespace no {{site.data.keyword.registryshort_notm}}. Os clusters existentes usam um [token](/docs/services/Registry?topic=registry-registry_access#registry_tokens) de registro que é armazenado em um cluster `imagePullSecret` para autorizar o acesso às imagens de extração por meio dos nomes de domínio `registry.bluemix.net`.
{:shortdesc}

Ao criar um cluster, segredos e tokens de registro sem expiração são criados automaticamente para o [registro regional mais próximo e o registro global](/docs/services/Registry?topic=registry-registry_overview#registry_regions). O registro global armazena com segurança as imagens públicas fornecidas pela IBM às quais é possível se referir em suas implementações em vez de ter referências diferentes para imagens que são armazenadas em cada registro regional. O registro regional armazena com segurança suas próprias imagens privadas do Docker. Os tokens são usados para autorizar o acesso somente leitura a qualquer um dos namespaces que você configurou no {{site.data.keyword.registryshort_notm}} para que seja possível trabalhar com essas imagens de registro públicas (registro global) e privadas (registro regional).

Cada token deve ser armazenado em um `imagePullSecret` do Kubernetes para que ele seja acessível para um cluster do Kubernetes quando você implementa um app conteinerizado. Quando o cluster é criado, o {{site.data.keyword.containerlong_notm}} armazena automaticamente os tokens para os registros globais (imagens públicas fornecidas pela IBM) e regionais nos segredos de extração de imagem do Kubernetes. Os segredos de extração de imagem são incluídos no namespace `default` do Kubernetes, no namespace `kube-system` e na lista de segredos na conta de serviço `default` para esses namespaces.

Esse método de usar um token para autorizar o acesso do cluster ao {{site.data.keyword.registrylong_notm}} é suportado para os nomes de domínio `registry.bluemix.net`, mas foi descontinuado. Em vez disso, [use o método da chave de API](#cluster_registry_auth) para autorizar o acesso do cluster aos novos nomes de domínio de registro `icr.io`.
{: deprecated}

Dependendo de onde a imagem estiver e onde o contêiner estiver, você deverá implementar contêineres seguindo diferentes etapas.
*   [Implementar um contêiner para o namespace `default` do Kubernetes com uma imagem que está na mesma região que seu cluster](#token_default_namespace)
*   [Implementar um contêiner em um namespace do Kubernetes diferente do `default`](#token_copy_imagePullSecret)
*   [Implementar um contêiner com uma imagem que está em uma região ou em uma conta do {{site.data.keyword.Bluemix_notm}} diferente do que seu cluster](#token_other_regions_accounts)
*   [Implementar um contêiner com uma imagem que é de um registro privado, não IBM](#private_images)

Usando essa configuração inicial, é possível implementar contêineres de qualquer imagem que esteja disponível em um namespace em sua conta do {{site.data.keyword.Bluemix_notm}} no namespace **padrão** de seu cluster. Para implementar um contêiner em outros namespaces de seu cluster ou para usar uma imagem que está armazenada em outra região do {{site.data.keyword.Bluemix_notm}} ou em outra conta do {{site.data.keyword.Bluemix_notm}}, deve-se [criar seu próprio segredo de extração de imagem para seu cluster](#other).
{: note}

### Descontinuado: implementando imagens para o namespace `default` do Kubernetes com um token de registro
{: #token_default_namespace}

Com o token de registro que está armazenado no segredo de extração da imagem, é possível implementar um contêiner de qualquer imagem que esteja disponível em seu {{site.data.keyword.registrylong_notm}} regional no namespace **padrão** de seu cluster.
{: shortdesc}

Antes de iniciar:
1. [Configure um namespace no {{site.data.keyword.registryshort_notm}} e envie por push as imagens para esse namespace](/docs/services/Registry?topic=registry-getting-started#gs_registry_namespace_add).
2. [Criar um cluster](/docs/containers?topic=containers-clusters#clusters_ui).
3. [Destine sua CLI para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

Para implementar um contêiner no namespace **padrão** de seu cluster, crie um arquivo de configuração.

1.  Crie um arquivo de configuração de implementação que é chamado de `mydeployment.yaml`.
2.  Defina a implementação e a imagem que você deseja usar por meio de seu namespace no {{site.data.keyword.registryshort_notm}}.

    Para usar uma imagem privada de um namespace no {{site.data.keyword.registryshort_notm}}:

    ```
    apiVersion: apps/v1 kind: Deployment metadata: name: <app_name>-deployment spec: replicas: 3 selector: matchLabels: app: <app_name> template: metadata: labels: app: <app_name> spec: containers:
          - name: <app_name>
            image: registry.<region>.bluemix.net/<namespace>/<my_image>:<tag>
    ```
    {: codeblock}

    **Dica:** para recuperar suas informações de namespace, execute `ibmcloud cr namespace-list`.

3.  Crie a implementação em seu cluster.

    ```
    kubectl apply -f mydeployment.yaml
    ```
    {: pre}

    **Dica:** também é possível implementar um arquivo de configuração existente, como uma das imagens públicas fornecidas pela IBM. Este exemplo usa a imagem **ibmliberty** na região sul dos EUA.

    ```
    kubectl apply -f https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/deploy-apps-clusters/deploy-ibmliberty.yaml
    ```
    {: pre}

### Descontinuado: copiando o segredo de extração da imagem baseada em token do namespace padrão para outros namespaces em seu cluster
{: #token_copy_imagePullSecret}

É possível copiar o segredo de extração da imagem com credenciais de token de registro que são criadas automaticamente para o namespace `default` do Kubernetes para outros namespaces em seu cluster.
{: shortdesc}

1. Liste namespaces disponíveis em seu cluster.
   ```
   kubectl get namespaces
   ```
   {: pre}

   Saída de exemplo:
   ```
   default          Active    79d
   ibm-cert-store   Active    79d
   ibm-system       Active    79d
   istio-system     Active    34d
   kube-public      Active    79d
   kube-system      Active    79d
   ```
   {: screen}

2. Opcional: crie um namespace em seu cluster.
   ```
   kubectl create namespace <namespace_name>
   ```
   {: pre}

3. Copie os segredos de extração de imagem do namespace `default` para o namespace de sua escolha. Os novos segredos de pull de imagem são nomeados `bluemix-<namespace_name>-secret-regional` e `bluemix-<namespace_name>-secret-international`.
   ```
   kubectl get secret bluemix-default-secret-regional -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

   ```
   kubectl get secret bluemix-default-secret-international -o yaml | sed 's/default/<namespace_name>/g' | kubectl -n <namespace_name> create -f -
   ```
   {: pre}

4.  Verifique se os segredos foram criados com êxito.
    ```
    kubectl get secrets --namespace <namespace_name>
    ```
    {: pre}

5. [Implementar um contêiner usando o `imagePullSecret`](#use_imagePullSecret) em seu namespace.


### Descontinuado: criando um segredo de extração de imagem baseado em token para acessar imagens em outras regiões e contas do {{site.data.keyword.Bluemix_notm}}
{: #token_other_regions_accounts}

Para acessar imagens em outras {{site.data.keyword.Bluemix_notm}} regiões ou contas, você deve criar um token de registro e salvar suas credenciais em uma imagem de pull secret.
{: shortdesc}

1.  Se você não tiver um token, [crie um token para o registro que você desejar acessar.](/docs/services/Registry?topic=registry-registry_access#registry_tokens_create)
2.  Liste tokens em sua conta. {{site.data.keyword.Bluemix_notm}}

    ```
    ibmcloud cr token-list
    ```
    {: pre}

3.  Anote o ID de token que você deseja usar.
4.  Recupere o valor para seu token. Substitua <em>&lt;token_ID&gt;</em> pelo ID do token que você recuperou na etapa anterior.

    ```
    ibmcloud cr token-get <token_id>
    ```
    {: pre}

    Seu valor do token é exibido no campo **Token** de sua saída da CLI.

5.  Crie o segredo do Kubernetes para armazenar suas informações do token.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_URL> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Entendendo os componentes deste comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace <em>&lt;kubernetes_namespace&gt;</em></code></td>
    <td>Necessário. O namespace do Kubernetes do cluster no qual você deseja usar o segredo e implementar contêineres. Execute <code>kubectl get namespaces</code> para listar todos os namespaces em seu cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;secret_name&gt;</em></code></td>
    <td>Necessário. O nome que você deseja usar para seu segredo de extração de imagem.</td>
    </tr>
    <tr>
    <td><code>--docker-server <em>&lt;registry_URL&gt;</em></code></td>
    <td>Necessário. A URL para o registro de imagem no qual o seu namespace está configurado.<ul><li>Para namespaces que são configurados no Sul e no Leste dos EUA <code>registry.ng.bluemix.net</code></li><li>Para namespaces que são configurados no Sul do Reino Unido <code>registry.eu-gb.bluemix.net</code></li><li>Para namespaces que são configurados na região central da UE (Frankfurt) <code>registry.eu-de.bluemix.net</code></li><li>Para namespaces que são configurados na Austrália (Sydney) <code>registry.au-syd.bluemix.net</code></li></ul></td>
    </tr>
    <tr>
    <td><code>--docker-username <em>&lt;docker_username&gt;</em></code></td>
    <td>Necessário. O nome do usuário para efetuar login no seu registro privado. Para o {{site.data.keyword.registryshort_notm}}, o nome do usuário é configurado como o valor <strong><code>token</code></strong>.</td>
    </tr>
    <tr>
    <td><code>--docker-password <em>&lt;token_value&gt;</em></code></td>
    <td>Necessário. O valor do token de registro que você recuperou anteriormente.</td>
    </tr>
    <tr>
    <td><code>--docker-email <em>&lt;docker-email&gt;</em></code></td>
    <td>Necessário. Se você tiver um, insira seu endereço de e-mail do Docker. Se não, insira um endereço de e-mail fictício, por exemplo a@b.c. Esse e-mail é obrigatório para criar um segredo do Kubernetes, mas não é usado após a criação.</td>
    </tr>
    </tbody></table>

6.  Verifique se o segredo foi criado com êxito. Substitua <em>&lt;kubernetes_namespace&gt;</em> pelo namespace no qual você criou o segredo de extração de imagem.

    ```
    kubectl get secrets --namespace <kubernetes_namespace>
    ```
    {: pre}

7.  [Implementar um contêiner usando o segredo de extração de imagem](#use_imagePullSecret) em seu namespace.
