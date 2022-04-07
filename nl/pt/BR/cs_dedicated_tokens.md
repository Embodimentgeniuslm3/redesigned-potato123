---

copyright:
  years: 2014, 2018
lastupdated: "2018-12-05"

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





# Criando um token do {{site.data.keyword.registryshort_notm}} para um registro de imagem do {{site.data.keyword.Bluemix_dedicated_notm}}
{: #cs_dedicated_tokens}

Crie um token sem expiração para um registro de imagem que você usou para grupos únicos e escaláveis com clusters no {{site.data.keyword.containerlong}}.
{:shortdesc}

1.  Solicite um token de registro permanente para a sessão atual. Esse token concede acesso às imagens no namespace atual.
    ```
    ibmcloud cr token-add --description "<description>" --non-expiring -q
    ```
    {: pre}

2.  Verifique o segredo do Kubernetes.

    ```
    kubectl describe secrets
    ```
    {: pre}

    É possível usar esse segredo para trabalhar com o {{site.data.keyword.containerlong}}.

3.  Crie o segredo do Kubernetes para armazenar suas informações do token.

    ```
    kubectl --namespace <kubernetes_namespace> create secret docker-registry <secret_name>  --docker-server=<registry_url> --docker-username=token --docker-password=<token_value> --docker-email=<docker_email>
    ```
    {: pre}

    <table>
    <caption>Entendendo os componentes deste comando</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes desse comando</th>
    </thead>
    <tbody>
    <tr>
    <td><code>--namespace &lt;kubernetes_namespace&gt;</code></td>
    <td>Necessário. O namespace do Kubernetes do cluster no qual você deseja usar o segredo e implementar contêineres. Execute <code>kubectl get namespaces</code> para listar todos os namespaces em seu cluster.</td>
    </tr>
    <tr>
    <td><code>&lt;secret_name&gt;</code></td>
    <td>Necessário. O nome que você deseja usar para seu imagePullSecret.</td>
    </tr>
    <tr>
    <td><code>--docker-server=&lt;registry_url&gt;</code></td>
    <td>Necessário. A URL para o registro de imagem no qual seu namespace está configurado: <code>registry.&lt;dedicated_domain&gt;</code></li></ul></td>
    </tr>
    <tr>
    <td><code> --docker-username=token </code></td>
    <td>Necessário. Não mude esse valor.</td>
    </tr>
    <tr>
    <td><code> --docker-password= &lt;token_value&gt; </code></td>
    <td>Necessário. O valor do token de registro que você recuperou anteriormente.</td>
    </tr>
    <tr>
    <td><code> --docker-email= &lt;docker-email&gt; </code></td>
    <td>Necessário. Se você tiver um, insira seu endereço de e-mail do Docker. Se não tiver um, insira um endereço de e-mail fictício, por exemplo a@b.c. Esse e-mail é obrigatório para criar um segredo do Kubernetes, mas não é usado após a criação.</td>
    </tr>
    </tbody></table>

4.  Crie um pod que referencie o imagePullSecret.

    1.  Abra seu editor de texto preferencial e crie um script de configuração de pod que seja chamado mypod.yaml.
    2.  Defina o pod e o imagePullSecret que você deseja usar para acessar o registro. Para usar uma imagem privada de um namespace:

        ```
        apiVersion: v1
        kind: Pod
        metadata:
          name: <pod_name>
        spec:
          containers:
            - name: <container_name>
              image: registry.<dedicated_domain>/<my_namespace>/<my_image>:<tag>
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
        <td><code>&lt;pod_name&gt;</code></td>
        <td>O nome do pod que você deseja criar.</td>
        </tr>
        <tr>
        <td><code>&lt;container_name&gt;</code></td>
        <td>O nome do contêiner que você deseja implementar em seu cluster.</td>
        </tr>
        <tr>
        <td><code>&lt;my_namespace&gt;</code></td>
        <td>O namespace no qual sua imagem está armazenada. Para listar os namespaces disponíveis, execute `ibmcloud cr namespace-list`.</td>
        </tr>
        <td><code>&lt;my_image&gt;</code></td>
        <td>O nome da imagem que você deseja usar. Para listar as imagens disponíveis em uma conta do {{site.data.keyword.Bluemix_notm}}, execute <code>ibmcloud cr image-list</code>.</td>
        </tr>
        <tr>
        <td><code>&lt;tag&gt;</code></td>
        <td>A versão da imagem que você deseja usar. Se nenhuma tag for especificada, a imagem identificada como <strong>mais recente</strong> será usada por padrão.</td>
        </tr>
        <tr>
        <td><code>&lt;secret_name&gt;</code></td>
        <td>O nome do imagePullSecret que você criou anteriormente.</td>
        </tr>
        </tbody></table>

    3.  Salve as suas mudanças.

    4.  Crie a implementação em seu cluster.

          ```
          kubectl apply -f mypod.yaml -n <namespace>
          ```
          {: pre}
