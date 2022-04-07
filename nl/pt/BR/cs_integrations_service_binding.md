---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

keywords: kubernetes, iks, helm, without tiller, private cluster tiller, integrations, helm chart

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



# Incluindo serviços com o uso da ligação de serviços do IBM Cloud
{: #service-binding}

Inclua serviços do {{site.data.keyword.Bluemix_notm}} para aprimorar o cluster do Kubernetes com recursos extras em áreas como Watson AI, dados, segurança e Internet of Things (IoT).
{:shortdesc}

**Quais tipos de serviços posso ligar ao meu cluster?**</br>
Quando você inclui serviços do {{site.data.keyword.Bluemix_notm}} em seu cluster, é possível escolher entre aqueles ativados para o {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) e aqueles baseados no Cloud Foundry. Os serviços ativados para o IAM oferecem um controle de acesso mais granular e podem ser gerenciados em um grupo de recursos do {{site.data.keyword.Bluemix_notm}}. Os serviços do Cloud Foundry devem ser incluídos em uma organização e em um espaço do Cloud Foundry e não podem ser incluídos em um grupo de recursos. Para controlar o acesso à sua instância de serviço do Cloud Foundry, use as funções do Cloud Foundry. Para obter mais informações sobre serviços ativados para o IAM e serviços do Cloud Foundry, consulte [O que é um recurso?](/docs/resources?topic=resources-resource#resource).

Para localizar uma lista de serviços suportados do {{site.data.keyword.Bluemix_notm}}, consulte o [catálogo do {{site.data.keyword.Bluemix_notm}}](https://cloud.ibm.com/catalog).

**O que é a ligação de serviços do {{site.data.keyword.Bluemix_notm}}?**</br>
A ligação de serviços é uma maneira rápida de criar credenciais de serviço para um serviço do {{site.data.keyword.Bluemix_notm}} e armazená-las em um segredo do Kubernetes em seu cluster. Para ligar um serviço ao cluster, deve-se primeiro provisionar uma instância do serviço. Em seguida, você usa o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_service_bind) `ibmcloud ks cluster-service-bind` para criar as credenciais de serviço e o segredo do Kubernetes. O segredo do Kubernetes é criptografado automaticamente em etcd para proteger seus dados.

Deseja tornar seus segredos ainda mais seguros? Peça ao administrador de cluster para [ativar o {{site.data.keyword.keymanagementservicefull}}](/docs/containers?topic=containers-encryption#keyprotect) em seu cluster para criptografar segredos novos e existentes, como o segredo que armazena as credenciais de suas instâncias de serviço do {{site.data.keyword.Bluemix_notm}}.
{: tip}

**Posso usar todos os serviços do {{site.data.keyword.Bluemix_notm}} em meu cluster?**</br>
É possível usar a ligação de serviços somente para serviços que suportem chaves de serviço, para que as credenciais de serviço possam ser criadas e armazenadas automaticamente em um segredo do Kubernetes. Para localizar uma lista de serviços que suportam chaves de serviço, consulte [Permitindo que aplicativos externos usem serviços do {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).

Os serviços que não suportam chaves de serviço geralmente fornecem uma API que pode ser usada em seu aplicativo. O método de ligação de serviços não configura automaticamente o acesso à API para seu aplicativo. Certifique-se de revisar a documentação da API de seu serviço e implementar nele a interface da API.

## Incluindo serviços do IBM Cloud em clusters
{: #bind-services}

Use a ligação de serviços do {{site.data.keyword.Bluemix_notm}} para criar automaticamente credenciais de serviço para seus serviços do {{site.data.keyword.Bluemix_notm}} e armazená-las em um segredo do Kubernetes.
{: shortdesc}

Antes de iniciar:
- Assegure-se de que você tenha as funções a seguir:
    - [Função **Editor** ou **Administrador** de acesso à plataforma {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o cluster no qual um serviço será ligado
    - [Função **Gravador** ou **Gerenciador** do serviço do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace do Kubernetes no qual o serviço será ligado
    - Para serviços do Cloud Foundry: [função **Desenvolvedor** do Cloud Foundry](/docs/iam?topic=iam-mngcf#mngcf) para o espaço no qual o serviço será provisionado
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Para incluir um serviço do {{site.data.keyword.Bluemix_notm}} em seu cluster:

1. [Crie uma instância do serviço {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).
    * Alguns serviços do {{site.data.keyword.Bluemix_notm}} estão disponíveis somente em regiões selecionadas. Será possível ligar um serviço a seu cluster somente se o serviço estiver disponível na mesma região que seu cluster. Além disso, se você deseja criar uma instância de serviço na zona Washington DC, deve-se usar a CLI.
    * **Para serviços ativados para o IAM**: deve-se criar a instância de serviço no mesmo grupo de recursos que o cluster. Um serviço poderá ser criado em apenas um grupo de recursos que não poderá ser mudado posteriormente.

2. Verifique o tipo de serviço que você criou e anote o **Nome** da instância de serviço.
   - **Serviços do Cloud Foundry:**
     ```
     Lista de serviços ibmcloud
     ```
     {: pre}

     Saída de exemplo:
     ```
     name                         service           plan    bound apps   last operation
     <cf_service_instance_name>   <service_name>    spark                create succeeded
     ```
     {: screen}

  - **Serviços ativados pelo {{site.data.keyword.Bluemix_notm}} IAM:**
     ```
     ibmcloud resource service-instances
     ```
     {: pre}

     Saída de exemplo:
     ```
     Name                          Location   State    Type               Tags
     <iam_service_instance_name>   <region>   active   service_instance
     ```
     {: screen}

   Também é possível ver os diferentes tipos de serviço no painel de seu {{site.data.keyword.Bluemix_notm}} como **Serviços do Cloud Foundry** e **Serviços**.

3. Identifique o espaço de nomes de cluster que você deseja usar para incluir o seu serviço.
   ```
   kubectl get namespaces
   ```
   {: pre}

4. Ligue o serviço a seu cluster para criar as credenciais de serviço para seu serviço e armazená-las em um segredo do Kubernetes. Se tiver credenciais de serviço existentes, use o sinalizador `--key` para especificá-las. Para serviços ativados para IAM, as credenciais são criadas automaticamente com a função de acesso ao serviço **Gravador**, mas é possível usar o sinalizador `--role` para especificar uma função de acesso de serviço diferente. Se você usar o sinalizador `--key`, não inclua o sinalizador `--role`.
   ```
   ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>] [--role <IAM_service_role>]
   ```
   {: pre}

   Quando a criação de credenciais de serviço for bem-sucedida, um segredo do Kubernetes com o nome `binding-<service_instance_name>` será criado.  

   Saída de exemplo:
   ```
   ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service cleardb
    Binding service instance to namespace...
    OK
    Namespace:	     mynamespace
    Secret name:     binding-<service_instance_name>
   ```
   {: screen}

5. Verifique as credenciais de serviço em seu segredo do Kubernetes.
   1. Obtenha os detalhes do segredo e anote o valor de **binding**. O valor de **binding** é codificado em Base64 e contém as credenciais para sua instância de serviço no formato JSON.
      ```
      kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
      ```
      {: pre}

      Saída de exemplo:
      ```
      apiVersion: v1
       data:
         binding: <binding>
       kind: Secret
       metadata:
         annotations:
           service-instance-id: 1111aaaa-a1aa-1aa1-1a11-111aa111aa11
           service-key-id: 2b22bb2b-222b-2bb2-2b22-b22222bb2222
         creationTimestamp: 2018-08-07T20:47:14Z
         name: binding-<service_instance_name>
         namespace: <namespace>
         resourceVersion: "6145900"
         selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
         uid: 33333c33-3c33-33c3-cc33-cc33333333c
       type: Opaque
      ```
      {: screen}

   2. Decode o valor de ligação.
      ```
      echo "<binding>" | base64 -D
      ```
      {: pre}

      Saída de exemplo:
      ```
      {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

   3. Opcional: compare as credenciais de serviço que você decodificou na etapa anterior com as credenciais de serviço localizadas para sua instância de serviço no painel do {{site.data.keyword.Bluemix_notm}}.

6. Agora que seu serviço está ligado a seu cluster, deve-se configurar seu app para [acessar as credenciais de serviço no segredo do Kubernetes](#adding_app).


## Acessando credenciais de serviço de seus apps
{: #adding_app}

Para acessar uma instância de serviço do {{site.data.keyword.Bluemix_notm}} por meio de seu app, deve-se tornar as credenciais de serviço que estão armazenadas no segredo do Kubernetes disponíveis para seu app.
{: shortdesc}

As credenciais de uma instância de serviço são codificadas em base64 e armazenadas dentro de seu segredo no formato JSON. Para acessar os dados em seu segredo, escolha entre as opções a seguir:
- [Montar o segredo como um volume para o pod](#mount_secret)
- [ Referenciar o segredo em variáveis de ambiente ](#reference_secret)
<br>

Antes de iniciar:
-  Assegure-se de que você tenha a [função de serviço **Gravador** ou **Gerenciador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace `kube-system`.
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- [ Inclua um serviço  {{site.data.keyword.Bluemix_notm}}  em seu cluster ](#bind-services).

### Montando o segredo como um volume em seu pod
{: #mount_secret}

Quando você monta o segredo como um volume em seu pod, um arquivo denominado `binding` é armazenado no diretório de montagem do volume. O arquivo `binding` em formato JSON inclui todas as informações e credenciais necessárias para acessar o serviço do {{site.data.keyword.Bluemix_notm}}.
{: shortdesc}

1.  Liste os segredos disponíveis em seu cluster e anote o **nome** de seu segredo. Procure um segredo do tipo **Opaco**. Se existirem múltiplos segredos, entre em contato com o administrador de cluster para identificar o segredo do serviço correto.
    ```
    kubectl get secrets
    ```
    {: pre}

    Saída de exemplo:
    ```
    NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
    ```
    {: screen}

2.  Crie um arquivo YAML para sua implementação do Kubernetes e monte o segredo como um volume em seu pod.
    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      labels:
        app: secret-test
      name: secret-test
      namespace: <my_namespace>
    spec:
      selector:
        matchLabels:
          app: secret-test
      replicas: 1
      template:
        metadata:
          labels:
            app: secret-test
        spec:
          containers:
          - image: icr.io/ibmliberty:latest
            name: secret-test
            volumeMounts:
            - mountPath: <mount_path>
              name: <volume_name>
          volumes:
          - name: <volume_name>
            secret:
              defaultMode: 420
              secretName: binding-<service_instance_name>
    ```
    {: codeblock}

    <table>
    <caption>Entendendo os componentes de arquivo YAML</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
    </thead>
    <tbody>
    <tr>
    <td><code> volumeMounts.mountPath </code></td>
    <td>O caminho absoluto do diretório no qual o volume está montado dentro do contêiner.</td>
    </tr>
    <tr>
    <td><code>volumeMounts.name</code></br><code>volumes.name</code></td>
    <td>O nome do volume a ser montado no pod.</td>
    </tr>
    <tr>
    <td><code> secret.defaultMode </code></td>
    <td>As permissões de leitura e gravação no segredo. Use  ` 420 `  para configurar permissões somente leitura. </td>
    </tr>
    <tr>
    <td><code> secret.secretName </code></td>
    <td>O nome do segredo que você anotou na etapa anterior.</td>
    </tr></tbody></table>

3.  Crie o pod e monte o segredo como um volume.
    ```
    Kubectl apply -f secret-test.yaml
    ```
    {: pre}

4.  Verifique se o pod foi criado.
    ```
    kubectl get pods
    ```
    {: pre}

    Exemplo de saída da CLI:

    ```
    NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
    ```
    {: screen}

5.  Acesse as credenciais de serviço.
    1. Efetue login em seu pod.
       ```
       kubectl exec < pod_name> -it bash
       ```
       {: pre}

    2. Navegue para o caminho de montagem do volume que você definiu anteriormente e liste os arquivos em seu caminho de montagem do volume.
       ```
       cd < volume_mountpath> & & ls
       ```
       {: pre}

       Saída de exemplo:
       ```
       ligando
       ```
       {: screen}

       O arquivo `binding` inclui as credenciais de serviço que você armazenou no segredo do Kubernetes.

    4. Visualize as credenciais de serviço. As credenciais são armazenadas como pares chave-valor em formato JSON.
       ```
       ligação cat
       ```
       {: pre}

       Saída de exemplo:
       ```
       {"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
       ```
       {: screen}

    5. Configure seu app para analisar o conteúdo JSON e recuperar as informações necessárias para acessar seu serviço.

### Referenciando o segredo em variáveis de ambiente
{: #reference_secret}

É possível incluir as credenciais de serviço e outros pares chave-valor de seu segredo do Kubernetes como variáveis de ambiente para a sua implementação.
{: shortdesc}

1. Liste os segredos disponíveis em seu cluster e anote o **nome** de seu segredo. Procure um segredo do tipo **Opaco**. Se existirem múltiplos segredos, entre em contato com o administrador de cluster para identificar o segredo do serviço correto.
   ```
   kubectl get secrets
   ```
   {: pre}

   Saída de exemplo:
   ```
   NAME                                    TYPE                                  DATA      AGE
    binding-<service_instance_name>         Opaque                                1         3m
   ```
   {: screen}

2. Obtenha os detalhes do seu segredo para localizar potenciais pares chave-valor que é possível referenciar como variáveis de ambiente em seu pod. As credenciais de serviço são armazenadas na chave `binding` de seu segredo.
   ```
   kubectl get secrets binding-<service_instance_name> --namespace=<namespace> -o yaml
   ```
   {: pre}

   Saída de exemplo:
   ```
   apiVersion: v1
   data:
     binding: <binding>
   kind: Secret
   metadata:
     annotations:
       service-instance-id: 7123acde-c3ef-4ba2-8c52-439ac007fa70
       service-key-id: 9h30dh8a-023f-4cf4-9d96-d12345ec7890
     creationTimestamp: 2018-08-07T20:47:14Z
     name: binding-<service_instance_name>
     namespace: <namespace>
     resourceVersion: "6145900"
     selfLink: /api/v1/namespaces/default/secrets/binding-mycloudant
     uid: 12345a31-9a83-11e8-ba83-cd49014748f
   type: Opaque
   ```
   {: screen}

3. Crie um arquivo YAML para a sua implementação do Kubernetes e especifique uma variável de ambiente que referencie a chave `binding`.
   ```
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     labels:
       app: secret-test
     name: secret-test
     namespace: <my_namespace>
   spec:
     selector:
       matchLabels:
         app: secret-test
     template:
       metadata:
         labels:
           app: secret-test
       spec:
         containers:
         - image: icr.io/ibmliberty:latest
           name: secret-test
           env:
           - name: BINDING
             valueFrom:
               secretKeyRef:
                 name: binding-<service_instance_name>
                 key: binding
     ```
     {: codeblock}

     <table>
     <caption>Entendendo os componentes de arquivo YAML</caption>
     <thead>
     <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
     </thead>
     <tbody>
     <tr>
     <td><code> containers.env.name </code></td>
     <td>O nome de sua variável de ambiente.</td>
     </tr>
     <tr>
     <td><code> env.valueFrom.secretKeyRef.name </code></td>
     <td>O nome do segredo que você anotou na etapa anterior.</td>
     </tr>
     <tr>
     <td><code> env.valueFrom.secretKeyRef.key </code></td>
     <td>A chave que faz parte de seu segredo e que você deseja referenciar em sua variável de ambiente. Para referenciar as credenciais de serviço, deve-se usar a chave <strong>binding</strong>.  </td>
     </tr>
     </tbody></table>

4. Crie o pod que referencia a chave `binding` de seu segredo como uma variável de ambiente.
   ```
   Kubectl apply -f secret-test.yaml
   ```
   {: pre}

5. Verifique se o pod foi criado.
   ```
   kubectl get pods
   ```
   {: pre}

   Exemplo de saída da CLI:
   ```
   NAME                           READY     STATUS    RESTARTS   AGE
    secret-test-1111454598-gfx32   1/1       Running   0          1m
   ```
   {: screen}

6. Verifique se a variável de ambiente está configurada corretamente.
   1. Efetue login em seu pod.
      ```
      kubectl exec < pod_name> -it bash
      ```
      {: pre}

   2. Liste todas as variáveis de ambiente no pod.
      ```
      env
      ```
      {: pre}

      Saída de exemplo:
      ```
      BINDING={"apikey":"KL34Ys893284NGJEPFjgrioJ12NElpow","host":"98765aab-9ce1-7tr3-ba87-bfbab6e6d9d6-bluemix.cloudant.com","iam_apikey_description":"Auto generated apikey during resource-key operation for Instance - crn:v1:bluemix:public:cloudantnosqldb:us-south:a/1234g56789cfe8e6388dd2ec098:98746cw-43d7-49ce-947a-d8fe3eebb381::","iam_apikey_name":"auto-generated-apikey-1234abcde-987f-3t64-9d96-d13775ec5663","iam_role_crn":"crn:v1:bluemix:public:iam::::serviceRole:Writer","iam_serviceid_crn":"crn:v1:bluemix:public:iam-identity::a/1234567890brasge5htn2ec098::serviceid:ServiceId-12345vgh-6c4c-ytr12-af6b-467d30d6ef44","password":"jfiavhui12484fnivhuo472nvei23913c3ff","port":443,"url":"https://25c73aac-9ce1-4c24-ba98-bfbab6e6d9d6-bluemix:ugvioev823inreuiegn43donvri29989wiu9t22@25c73aac-9ce1-4c24-ba98-abdrjio123562lnsb-bluemix.cloudant.com","username":"123b45da-9ce1-4c24-ab12-rinwnwub1294-bluemix"}
      ```
      {: screen}

7. Configure seu app para ler a variável de ambiente e para analisar o conteúdo JSON para recuperar as informações que você precisa para acessar seu serviço.

   Código de exemplo em Python:
   ```python
   if os.environ.get('BINDING'):
        credentials = json.loads(os.environ.get('BINDING'))
   ```
   {: codeblock}

8. Opcional: como uma precaução, inclua a manipulação de erros em seu aplicativo, caso a variável de ambiente `BINDING` não esteja configurada corretamente.

   Código de exemplo em Java:
   ```java
   if (System.getenv("BINDING") == null) {
    throw new RuntimeException("Environment variable 'SECRET' is not set!");
   }
   ```
   {: codeblock}

   Código de exemplo em Node.js:
   ```js
   if (!process.env.BINDING) {
    console.error('ENVIRONMENT variable "BINDING" is not set!');
    process.exit(1);
   }
   ```
   {: codeblock}
