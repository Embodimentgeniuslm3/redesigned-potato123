---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-05"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Resolução de problemas de armazenamento do cluster
{: #cs_troubleshoot_storage}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas de armazenamento em cluster.
{: shortdesc}

Se você tiver um problema mais geral, tente a [depuração do cluster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}


## Depurando falhas de armazenamento persistente
{: #debug_storage}

Revise as opções para depurar o armazenamento persistente e localizar as causas raiz para falhas.
{: shortdesc}

1. Verifique se você usa a versão mais recente do {{site.data.keyword.Bluemix_notm}} e do plug-in do {{site.data.keyword.containerlong_notm}}. 
   ```
   Atualizar ibmcloud
   ```
   {: pre}
   
   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. Verifique se a versão da CLI `kubectl` que você executa em sua máquina local corresponde à versão do Kubernetes que está instalada em seu cluster. 
   1. Mostre a versão da CLI `kubectl` que está instalada em seu cluster e em sua máquina local.
      ```
      kubectl version
      ```
      {: pre} 
   
      Saída de exemplo: 
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}
    
      As versões da CLI corresponderão se você puder ver a mesma versão em `GitVersion` para o cliente e o servidor. É possível ignorar a parte `+IKS` da versão para o servidor.
   2. Se a versão da CLI `kubectl` em sua máquina local e seu cluster não corresponderem, [atualize seu cluster](/docs/containers?topic=containers-update) ou [instale uma versão da CLI diferente em sua máquina local](/docs/containers?topic=containers-cs_cli_install#kubectl). 

3. Somente para armazenamento de bloco, armazenamento de objeto e Portworx: certifique-se de que você [tenha instalado o servidor do Helm, o Tiller, com uma conta de serviços do Kubernetes](/docs/containers?topic=containers-helm#public_helm_install). 

4. Somente para armazenamento de bloco, armazenamento de objeto e Portworx: certifique-se de que você tenha instalado a versão mais recente do gráfico do Helm para o plug-in. 
   
   **Armazenamento de bloco e objeto**: 
   
   1. Atualize seus repositórios de gráfico do Helm.
      ```
      helm repo update
      ```
      {: pre}
      
   2. Liste os gráficos do Helm no repositório `iks-charts`.
      ```
      helm search iks-charts
      ```
      {: pre}
      
      Saída de exemplo: 
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}
      
   3. Liste os gráficos do Helm instalados em seu cluster e compare a versão instalada com a versão que está disponível. 
      ```
      helm ls
      ```
      {: pre}
      
   4. Se uma versão mais recente estiver disponível, instale-a. Para obter instruções, consulte [Atualizando o plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in) e [Atualizando o plug-in do {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#update_cos_plugin). 
   
   **Portworx**: 
   
   1. Localize a [versão mais recente do gráfico do Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/portworx/helm/tree/master/charts/portworx) que está disponível 
   
   2. Liste os gráficos do Helm instalados em seu cluster e compare a versão instalada com a versão que está disponível. 
      ```
      helm ls
      ```
      {: pre}
   
   3. Se uma versão mais recente estiver disponível, instale-a. Para obter instruções, consulte [Atualizando o Portworx em seu cluster](/docs/containers?topic=containers-portworx#update_portworx). 
   
5. Verifique se o driver de armazenamento e os pods de plug-in mostram um status de **Em execução**. 
   1. Liste os pods no namespace `kube-system`.
      ```
      kubectl get pods -n kube-system 
      ```
      {: pre}
      
   2. Se os pods não mostrarem um status **Em execução**, obtenha mais detalhes do pod para localizar a causa raiz. Dependendo do status de seu pod, você pode não ser capaz de executar todos os comandos a seguir.
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}
      
      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}
      
   3. Analise a seção **Eventos** da saída da CLI do comando `kubectl describe pod` e os logs mais recentes para localizar a causa raiz para o erro. 
   
6. Verifique se a PVC foi provisionada com êxito. 
   1. Verifique o estado da sua PVC. Uma PVC será provisionada com êxito se a PVC mostrar um status **Ligado**.
      ```
      kubectl get pvc
      ```
      {: pre}
      
   2. Se o estado da PVC mostrar **Pendente**, recupere o erro que explica por que a PVC permanece pendente.
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}
      
   3. Revise erros comuns que podem ocorrer durante a criação da PVC. 
      - [Armazenamento de arquivo e armazenamento de bloco: a PVC permanece em um estado pendente](#file_pvc_pending)
      - [Armazenamento de objeto: a PVC permanece em um estado pendente](#cos_pvc_pending)
   
7. Verifique se o pod que monta sua instância de armazenamento foi implementado com êxito. 
   1. Liste os pods em seu cluster. Um pod será implementado com êxito se o pod mostrar um status de **Em execução**.
      ```
      kubectl get pods
      ```
      {: pre}
      
   2. Obtenha os detalhes de seu pod e verifique se os erros são exibidos na seção **Eventos** de sua saída da CLI.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}
   
   3. Recupere os logs para seu app e verifique se é possível ver quaisquer mensagens de erro.
      ```
      kubectl logs <pod_name>
      ```
      {: pre}
   
   4. Revise erros comuns que podem ocorrer ao montar uma PVC em seu app. 
      - [Armazenamento de arquivo: o app não pode acessar nem gravar na PVC](#file_app_failures)
      - [Armazenamento de bloco: o app não pode acessar nem gravar na PVC](#block_app_failures)
      - [Armazenamento de objeto: falha ao acessar arquivos com um usuário não raiz](#cos_nonroot_access)
      

## Armazenamento de arquivo e armazenamento de bloco: a PVC permanece em um estado pendente
{: #file_pvc_pending}

{: tsSymptoms}
Quando você cria uma PVC e executa o `kubectl get pvc <pvc_name>`, sua PVC permanece em um estado **Pendente**, mesmo depois de esperar por algum tempo. 

{: tsCauses}
Durante a criação e a ligação da PVC, muitas tarefas diferentes são executadas pelo plug-in de armazenamento de arquivo e de bloco. Cada tarefa pode falhar e causar uma mensagem de erro diferente.

{: tsResolve}

1. Localize a causa raiz para saber o motivo de o PVC permanecer em um estado **Pendente**. 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Revise as descrições e as resoluções comuns da mensagem de erro.
   
   <table>
   <thead>
     <th>A mensagem de erro</th>
     <th>Descrição</th>
     <th>Etapas a serem resolvidas</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>A chave de API do IAM ou a chave de API de infraestrutura do IBM Cloud (SoftLayer) que está armazenada no segredo do Kubernetes `storage-secret-store` de seu cluster não tem todas as permissões necessárias para provisionar armazenamento persistente. </td>
      <td>Consulte [A criação do PVC falha devido a permissões ausentes](#missing_permissions). </td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>Cada conta do {{site.data.keyword.Bluemix_notm}} é configurada com um número máximo de instâncias de armazenamento que podem ser criadas. Criando a PVC, você excede o número máximo de instâncias de armazenamento. </td>
      <td>Para criar uma PVC, escolha dentre as opções a seguir. <ul><li>Remover quaisquer PVCs não usadas.</li><li>Solicitar ao proprietário da conta do {{site.data.keyword.Bluemix_notm}} para aumentar sua cota de armazenamento, [abrindo um caso de suporte](/docs/get-support?topic=get-support-getting-customer-support).</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>O tamanho do armazenamento e o IOPS que você especificou em sua PVC não são suportados pelo tipo de armazenamento escolhido e não podem ser usados com a classe de armazenamento especificada. </td>
      <td>Revise [Decidindo sobre a configuração de armazenamento de arquivo](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) e [Decidindo sobre a configuração de armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para localizar tamanhos de armazenamento e IOPS suportados para a classe de armazenamento que você deseja usar. Corrija o tamanho e o IOPS e recrie a PVC. </td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>O data center que você especificou em sua PVC não existe. </td>
  <td>Execute <code>ibmcloud ks locations</code> para listar os data centers disponíveis. Corrija o data center em sua PVC e recrie a PVC.</td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>O tamanho do armazenamento, o IOPS e o tipo de armazenamento podem ser incompatíveis com a classe de armazenamento escolhida ou o terminal de API de infraestrutura do {{site.data.keyword.Bluemix_notm}} está atualmente indisponível. </td>
  <td>Revise [Decidindo sobre a configuração de armazenamento de arquivo](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) e [Decidindo sobre a configuração de armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para localizar tamanhos de armazenamento e IOPS suportados para a classe de armazenamento e o tipo de armazenamento que você deseja usar. Em seguida, exclua e recrie o PVC. </td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>Você deseja criar uma PVC para uma instância de armazenamento existente usando o provisionamento estático de Kubernetes, mas a instância de armazenamento especificada não pôde ser localizada. </td>
  <td>Siga as instruções para provisionar [armazenamento de arquivo](/docs/containers?topic=containers-file_storage#existing_file) ou [armazenamento de bloco](/docs/containers?topic=containers-block_storage#existing_block) existente em seu cluster e certifique-se de recuperar as informações corretas para sua instância de armazenamento existente. Em seguida, exclua e recrie o PVC.  </td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>Você criou uma classe de armazenamento customizado e especificou um tipo de armazenamento que não é suportado.</td>
  <td>Atualize sua classe de armazenamento customizada para especificar `Endurance` ou `Performance` como seu tipo de armazenamento. Para localizar exemplos para classes de armazenamento customizado, consulte as classes de armazenamento customizado de amostra para [armazenamento de arquivo](/docs/containers?topic=containers-file_storage#file_custom_storageclass) e [armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_custom_storageclass). </td>
  </tr>
  </tbody>
  </table>
  
## Armazenamento de arquivo: o app não pode acessar nem gravar na PVC
{: #file_app_failures}

Ao montar um PVC em seu pod, é possível ter erros ao acessar ou gravar no PVC. 
{: shortdesc}

1. Liste os pods em seu cluster e revise seus status. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Localize a causa raiz para saber o motivo de seu app não poder acessar ou gravar no PVC.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Revise os erros comuns que podem ocorrer ao montar um PVC em um pod. 
   <table>
   <thead>
     <th>Sintoma ou mensagem de erro</th>
     <th>Descrição</th>
     <th>Etapas a serem resolvidas</th>
  </thead>
  <tbody>
    <tr>
      <td>Seu pod está preso em um estado <strong>ContainerCreating</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>O back-end de infraestrutura do {{site.data.keyword.Bluemix_notm}} passou por problemas de rede. Para proteger seus dados e para evitar distorção de dados, o {{site.data.keyword.Bluemix_notm}} desconectou automaticamente o servidor de armazenamento de arquivo para evitar operações de gravação em compartilhamentos de arquivo NFS.  </td>
      <td>Consulte [Armazenamento de arquivo: os sistemas de arquivos para os nós do trabalhador mudam para somente leitura](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>Em sua implementação, você especificou um usuário não raiz para possuir o caminho de montagem do armazenamento de arquivo NFS. Por padrão, os usuários não raiz não tem permissão de gravação no caminho de montagem do volume para armazenamento suportado por NFS. </td>
  <td>Consulte [Armazenamento de arquivo: o app falha quando um usuário não raiz possui o caminho de montagem do armazenamento de arquivo NFS](#nonroot)</td>
  </tr>
  <tr>
  <td>Depois de ter especificado um usuário não raiz para possuir o caminho de montagem do armazenamento de arquivo NFS ou implementado um gráfico do Helm com um ID do usuário não raiz especificado, o usuário não poderá gravar no armazenamento montado.</td>
  <td>A implementação ou a configuração do gráfico o Helm especifica o contexto de segurança para o <code>fsGroup</code> (ID do grupo) e <code>runAsUser</code> (ID do usuário) do pod</td>
  <td>Consulte [Armazenamento de arquivo: a inclusão do acesso de usuário não raiz no armazenamento persistente falha](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### Armazenamento de arquivo: os sistemas de arquivos para nós do trabalhador mudam para somente leitura
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
Você pode ver um dos sintomas a seguir:
- Quando você executa `kubectl get pods -o wide`, você vê que vários pods que estão em execução no mesmo nó do trabalhador estão presos no estado `ContainerCreating`.
- Ao executar um comando `kubectl describe`, você vê o erro a seguir na seção **Eventos**: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
O sistema de arquivos no nó do trabalhador é somente leitura.

{: tsResolve}
1.  Faça backup de quaisquer dados que podem ser armazenados no nó do trabalhador ou em seus contêineres.
2.  Para uma correção de curto prazo para o nó do trabalhador existente, recarregue o nó do trabalhador.
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

Para uma correção de longo prazo, [atualize o tipo de máquina do seu conjunto de trabalhadores](/docs/containers?topic=containers-update#machine_type).

<br />



### Armazenamento de arquivo: o app falha quando um usuário não raiz possui o caminho de montagem do armazenamento de arquivo NFS
{: #nonroot}

{: tsSymptoms}
Depois de [incluir armazenamento NFS](/docs/containers?topic=containers-file_storage#file_app_volume_mount) em sua implementação, a implementação de seu contêiner falha. Ao recuperar os logs para seu contêiner, é possível ver erros como os seguintes. O pod falha e fica preso em um ciclo de recarregamento.

```
write-permission
```
{: screen}

```
não possui permissão necessária
```
{: screen}

```
não é possível criar o diretório '/bitnami/mariadb/data': Permissão negada
```
{: screen}

{: tsCauses}
Por padrão, os usuários não raiz não tem permissão de gravação no caminho de montagem do volume para armazenamento suportado por NFS. Algumas imagens comuns do app, como Jenkins e Nexus3, especificam um usuário não raiz que possui o caminho de montagem no Dockerfile. Quando você cria um contêiner por meio desse Dockerfile, a criação do contêiner falha devido a permissões insuficientes do usuário não raiz no caminho de montagem. Para conceder permissão de gravação, é possível modificar o Dockerfile para incluir temporariamente o usuário não raiz no grupo de usuários raiz antes que ele mude as permissões de caminho de montagem ou usar um contêiner de inicialização.

Se você usar um gráfico Helm para implementar a imagem, edite a implementação do Helm para usar um contêiner de inicialização.
{:tip}



{: tsResolve}
Quando você inclui um [contêiner de inicialização![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) em sua implementação, é possível fornecer a um usuário não raiz que está especificado em seu Dockerfile as permissões de gravação para o caminho de montagem do volume dentro do contêiner. O contêiner de inicialização é iniciado antes de seu contêiner de app ser iniciado. O contêiner de inicialização cria o caminho de montagem do volume dentro do contêiner, muda o caminho de montagem para ser de propriedade do usuário correto (não raiz) e fecha. Em seguida, seu contêiner de app é iniciado com o usuário não raiz que deve gravar no caminho de montagem. Como o caminho já pertence ao usuário não raiz, a gravação no caminho de montagem é bem-sucedida. Se você não deseja usar um contêiner de inicialização, é possível modificar o Dockerfile para incluir acesso de usuário não raiz no armazenamento de arquivo NFS.


Antes de iniciar: [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Abra o Dockerfile para seu app e obtenha o ID do usuário (UID) e ID do grupo (GID) do usuário que você deseja fornecer permissão de gravação no caminho de montagem do volume. No exemplo de um Dockerfile Jenkins, as informações são:
    - UID: `1000`
    - GID: `1000`

    **Exemplo**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update &&apt-get install -y git curl &&rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Inclua armazenamento persistente para seu app criando um persistent volume claim (PVC). Este exemplo usa a classe de armazenamento `ibmc-file-bronze`. Para revisar as classes de armazenamento disponíveis, execute `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Crie o PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  Em seu arquivo de implementação `.yaml`, inclua o contêiner de inicialização. Inclua o UID e o GID que você recuperou anteriormente.

    ```
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Exemplo** para uma implementação Jenkins:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Crie o pod e monte o PVC em seu pod.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verifique se o volume foi montado com êxito no pod. Anote o nome do pod e o caminho **Contêineres/Montagens**.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Saída de exemplo**:

    ```
    Name:		    mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type: PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName: mypvc
        ReadOnly: false

    ```
    {: screen}

7.  Efetue login no pod usando o nome do pod que você anotou anteriormente.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verifique as permissões do caminho de montagem do seu contêiner. No exemplo, o caminho de montagem é `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Saída de exemplo**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    Essa saída mostra que o GID e o UID de seu Dockerfile (neste exemplo, `1000` e `1000`) possuem o caminho de montagem dentro do contêiner.

<br />


### Armazenamento de arquivo: a inclusão do acesso de usuário não raiz no armazenamento persistente falha
{: #cs_storage_nonroot}

{: tsSymptoms}
Depois de [incluir o acesso de usuário não raiz ao armazenamento persistente](#nonroot) ou implementar um gráfico do Helm com um ID do usuário não raiz especificado, o usuário não pode gravar no armazenamento montado.

{: tsCauses}
A configuração da implementação ou do gráfico Helm especifica o [contexto de segurança](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) para o `fsGroup` (ID do grupo) e `runAsUser` (ID do usuário) do pod. Atualmente, o {{site.data.keyword.containerlong_notm}} não suporta a especificação `fsGroup` e suporta somente `runAsUser` configurado como `0` (permissões raiz).

{: tsResolve}
Remova os campos `securityContext` da configuração para `fsGroup` e `runAsUser` do arquivo de configuração de imagem, implementação ou gráfico Helm e reimplemente. Se você precisa mudar a propriedade do caminho de montagem de `nobody`, [inclua o acesso de usuário não raiz](#nonroot). Depois de incluir o [non-root `initContainer`](#nonroot), configure `runAsUser` no nível do contêiner, não no nível do pod.

<br />




## Armazenamento de bloco: o app não pode acessar nem gravar na PVC
{: #block_app_failures}

Ao montar um PVC em seu pod, é possível ter erros ao acessar ou gravar no PVC. 
{: shortdesc}

1. Liste os pods em seu cluster e revise seus status. 
   ```
   kubectl get pods
   ```
   {: pre}
   
2. Localize a causa raiz para saber o motivo de seu app não poder acessar ou gravar no PVC.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}
   
   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Revise os erros comuns que podem ocorrer ao montar um PVC em um pod. 
   <table>
   <thead>
     <th>Sintoma ou mensagem de erro</th>
     <th>Descrição</th>
     <th>Etapas a serem resolvidas</th>
  </thead>
  <tbody>
    <tr>
      <td>Seu pod está preso em um estado <strong>ContainerCreating</strong> ou <strong>CrashLoopBackOff</strong>. </br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>O back-end de infraestrutura do {{site.data.keyword.Bluemix_notm}} passou por problemas de rede. Para proteger seus dados e para evitar distorção de dados, o {{site.data.keyword.Bluemix_notm}} desconectou automaticamente o servidor de armazenamento de bloco para evitar operações de gravação em instâncias de armazenamento de bloco.  </td>
      <td>Consulte [Armazenamento de bloco: o armazenamento de bloco muda para somente leitura](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>Você deseja montar uma instância de armazenamento de bloco existente que foi configurada com um sistema de arquivos <code>XFS</code>. Quando criou o PV e a PVC correspondente, você especificou um <code>ext4</code> ou nenhum sistema de arquivos. O sistema de arquivos especificado em seu PV deve ser o mesmo sistema de arquivos que está configurado em sua instância de armazenamento de bloco. </td>
  <td>Consulte [Armazenamento de bloco: a montagem do armazenamento de bloco existente em um pod falha devido ao sistema de arquivos errado](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### Armazenamento de bloco: o armazenamento de bloco muda para somente leitura
{: #readonly_block}

{: tsSymptoms}
É possível ver os sintomas a seguir:
- Ao executar `kubectl get pods -o wide`, você vê que múltiplos pods no mesmo nó do trabalhador estão presos no estado `ContainerCreating` ou `CrashLoopBackOff`. Todos esses pods usam a mesma instância de armazenamento de bloco.
- Quando você executa um comando `kubectl describe pod`, você vê o seguinte erro na seção **Eventos**: `MountVolume.SetUp falhou para o volume ... de leitura`.

{: tsCauses}
Se ocorrer um erro de rede enquanto um pod gravar em um volume, a infraestrutura do IBM Cloud (SoftLayer) protegerá os dados no volume de serem corrompidos mudando o volume para um modo somente leitura. Os pods que usam esse volume não podem continuar a gravar no volume e falham.

{: tsResolve}
1. Verifique a versão do plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage que está instalado em seu cluster.
   ```
   helm ls
   ```
   {: pre}

2. Verifique se você usa a [versão mais recente do plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin). Se não,  [ atualize seu plug-in ](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in).
3. Se você usou uma implementação do Kubernetes para seu pod, reinicie o pod que está falhando removendo-o e permitindo que o Kubernetes o recrie. Se você não usou uma implementação, recupere o arquivo YAML usado para criar seu pod executando `kubectl get pod <pod_name> -o yaml >pod.yaml`. Em seguida, exclua e recrie manualmente o pod.
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. Verifique se a recriação de seu pod resolveu o problema. Se não estiver, recarregue o nó do trabalhador.
   1. Localize o nó do trabalhador no qual seu pod é executado e anote o endereço IP privado que está designado ao seu nó do trabalhador.
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      Saída de exemplo:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. Recupere o **ID** do nó do trabalhador usando o endereço IP privado da etapa anterior.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. Gracentemente [recarregue o nó do trabalhador](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).


<br />


### Armazenamento de bloco: a montagem do armazenamento de bloco existente em um pod falha devido ao sistema de arquivo errado
{: #block_filesystem}

{: tsSymptoms}
Ao executar `kubectl describe pod <pod_name>`, você verá o erro a seguir:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
Você tem um dispositivo de armazenamento de bloco existente que é configurado com um sistema de arquivos `XFS`. Para montar esse dispositivo em seu pod, você [criou um PV](/docs/containers?topic=containers-block_storage#existing_block) que especificou `ext4` como seu sistema de arquivos ou nenhum sistema de arquivos na seção `spec/flexVolume/fsType`. Se nenhum sistema de arquivos está definido, o PV é padronizado para `ext4`.
O PV foi criado com êxito e foi vinculado à sua instância de armazenamento de bloco existente. No entanto, quando você tenta montar o PV em seu cluster usando um PVC correspondente, o volume falha ao ser montado. Não é possível montar sua instância de armazenamento de bloco `XFS` com um sistema de arquivos `ext4` no pod.

{: tsResolve}
Atualize o sistema de arquivos no PV existente de `ext4` para `XFS`.

1. Liste os PVs existentes em seu cluster e anote o nome do PV que você usou para a sua instância de armazenamento de bloco existente.
   ```
   kubectl get pv
   ```
   {: pre}

2. Salve o YAML do PV em sua máquina local.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Abra o arquivo YAML e mude o `fsType` de `ext4` para `xfs`.
4. Substitua o PV em seu cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Efetue login no pod no qual você montou o PV.
   ```
   Kubectl exec -it < pod_name> sh
   ```
   {: pre}

6. Verifique se o sistema de arquivos mudou para `XFS`.
   ```
   -th df
   ```
   {: pre}

   Saída de exemplo:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Armazenamento de objeto: a instalação do plug-in Helm `ibmc` do {{site.data.keyword.cos_full_notm}} falha
{: #cos_helm_fails}

{: tsSymptoms}
Quando você instala o plug-in do Helm {{site.data.keyword.cos_full_notm}}`ibmc`, a instalação falha com um dos erros a seguir:
```
Error: symlink /Users/ibm/ibmcloud-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
Quando o plug-in Helm `ibmc` está instalado, um symlink é criado do diretório `./helm/plugins/helm-ibmc` para o diretório no qual o plug-in Helm `ibmc` está localizado em seu sistema local, que está geralmente em `./ibmcloud-object-storage-plugin/helm-ibmc`. Quando você remove o plug-in Helm `ibmc` de seu sistema local ou move o diretório do plug-in Helm `ibmc` para um local diferente, o symlink não é removido.

Caso veja um erro `permission denied`, você não tem as permissões necessárias `read`, `write` e `execute` no arquivo bash `ibmc.sh` para que seja possível executar comandos do plug-in do Helm `ibmc`. 

{: tsResolve}

**Para erros de symlink**: 

1. Remova o  {{site.data.keyword.cos_full_notm}}  Plug-in do Helm.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. Siga a [documentação](/docs/containers?topic=containers-object_storage#install_cos) para reinstalar o plug-in do Helm `ibmc` e o plug-in do {{site.data.keyword.cos_full_notm}}.

**Para erros de permissão**: 

1. Mude as permissões para o plug-in `ibmc`. 
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}
   
2. Experimente o plug-in do Helm do `ibm`. 
   ```
   helm ibmc -- help
   ```
   {: pre}
   
3. [Continue instalando o plug-in do {{site.data.keyword.cos_full_notm}}](/docs/containers?topic=containers-object_storage#install_cos). 


<br />


## Armazenamento de objeto: a PVC permanece em um estado pendente
{: #cos_pvc_pending}

{: tsSymptoms}
Quando você cria uma PVC e executa o `kubectl get pvc <pvc_name>`, sua PVC permanece em um estado **Pendente**, mesmo depois de esperar por algum tempo. 

{: tsCauses}
Durante a criação e a ligação da PVC, muitas tarefas diferentes são executadas pelo plug-in do {{site.data.keyword.cos_full_notm}}. Cada tarefa pode falhar e causar uma mensagem de erro diferente.

{: tsResolve}

1. Localize a causa raiz para saber o motivo de o PVC permanecer em um estado **Pendente**. 
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}
   
2. Revise as descrições e as resoluções comuns da mensagem de erro.
   
   <table>
   <thead>
     <th>A mensagem de erro</th>
     <th>Descrição</th>
     <th>Etapas a serem resolvidas</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>A chave de API do IAM ou a chave de API de infraestrutura do IBM Cloud (SoftLayer) que está armazenada no segredo do Kubernetes `storage-secret-store` de seu cluster não tem todas as permissões necessárias para provisionar armazenamento persistente. </td>
      <td>Consulte [A criação do PVC falha devido a permissões ausentes](#missing_permissions). </td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>O segredo do Kubernetes que retém suas credenciais de serviço do {{site.data.keyword.cos_full_notm}} não existe no mesmo namespace que a PVC ou o pod. </td>
      <td>Consulte [A criação da PVC ou do pod falha devido à não localização do segredo do Kubernetes](#cos_secret_access_fails).</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>O segredo do Kubernetes que você criou para sua instância de serviço do {{site.data.keyword.cos_full_notm}} não inclui o `type: ibm/ibmc-s3fs`.</td>
      <td>Edite o segredo do Kubernetes que retém suas credenciais do {{site.data.keyword.cos_full_notm}} para incluir ou mudar o `type` para `ibm/ibmc-s3fs`. </td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>O terminal de API do s3fs ou da API do IAM tem o formato errado ou o terminal de API do s3fs não pôde ser recuperado com base em seu local do cluster.  </td>
      <td>Veja [A criação da PVC falha devido ao terminal de API do s3fs errado](#cos_api_endpoint_failure).</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>Você especificou um subdiretório existente em seu depósito que deseja montar em seu PVC usando a anotação `ibm.io/object-path`. Se você configura um subdiretório, deve-se desativar o recurso de criação automática de depósito.  </td>
      <td>Em sua PVC, configure `ibm.io/auto-create-bucket: "false"` e forneça o nome do depósito existente em `ibm.io/bucket`.</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>No PVC, configure `ibm.io/auto-delete-bucket: true` para excluir automaticamente seus dados, o depósito e o PV, ao remover o PVC. Essa opção requer que `ibm.io/auto-create-bucket` seja configurado como <strong>true</strong> e `ibm.io/bucket` seja configurado como `""` ao mesmo tempo.</td>
    <td>No PVC, configure `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""` para que seu depósito seja criado automaticamente com um nome no formato `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>No PVC, configure `ibm.io/auto-delete-bucket: true` para excluir automaticamente seus dados, o depósito e o PV, ao remover o PVC. Essa opção requer que `ibm.io/auto-create-bucket` seja configurado como <strong>true</strong> e `ibm.io/bucket` seja configurado como `""` ao mesmo tempo.</td>
    <td>No PVC, configure `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""` para que seu depósito seja criado automaticamente com um nome no formato `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>Se você deseja usar as chaves da API do IAM para acessar sua instância de serviço do {{site.data.keyword.cos_full_notm}}, deve-se armazenar a chave de API e o ID da instância de serviço do {{site.data.keyword.cos_full_notm}} em um segredo do Kubernetes.  </td>
    <td>Consulte [Criando um segredo para as credenciais do serviço de armazenamento de objeto](/docs/containers?topic=containers-object_storage#create_cos_secret). </td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>Você especificou um subdiretório existente em seu depósito que deseja montar em seu PVC usando a anotação `ibm.io/object-path`. Esse subdiretório não pôde ser localizado no depósito que você especificou. </td>
      <td>Verifique se o subdiretório que você especificou em `ibm.io/object-path` existe no depósito especificado em `ibm.io/bucket`. </td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>Você configurou `ibm.io/auto-create-bucket: true` e especificou um nome de depósito ao mesmo tempo ou especificou um nome de depósito que já existe no {{site.data.keyword.cos_full_notm}}. Os nomes de depósito devem ser exclusivos em todas as instâncias de serviço e regiões no {{site.data.keyword.cos_full_notm}}.  </td>
          <td>Certifique-se de configurar `ibm.io/auto-create-bucket: false` e fornecer um nome de depósito que seja exclusivo no {{site.data.keyword.cos_full_notm}}. Se você desejar usar o plug-in do {{site.data.keyword.cos_full_notm}} para criar automaticamente um nome de depósito, configure `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""`. Seu depósito é criado com um nome exclusivo no formato `tmp-s3fs-xxxx`. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>Você tentou acessar um depósito que não foi criado ou a classe de armazenamento e o terminal de API do s3fs especificado não correspondem à classe de armazenamento e ao terminal de API do s3fs que foram usados quando o depósito foi criado. </td>
          <td>Consulte [Não é possível acessar um depósito existente](#cos_access_bucket_fails). </td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>Os valores em seu segredo do Kubernetes não estão codificados corretamente para base64.  </td>
          <td>Revise os valores em seu segredo do Kubernetes e codifique cada valor para base64. Também é possível usar o comando [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) para criar um novo segredo e permitir que o Kubernetes codifique automaticamente seus valores para base64. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>Você especificou `ibm.io/auto-create-bucket: false` e tentou acessar um depósito que não foi criado ou a chave de acesso ao serviço ou o ID da chave de acesso de suas credenciais do {{site.data.keyword.cos_full_notm}} HMAC estão incorretos.  </td>
          <td>Não é possível acessar um depósito que você não criou. Crie um novo depósito em vez de configurar `ibm.io/auto-create-bucket: true` e `ibm.io/bucket: ""`. Se você for o proprietário do depósito, veja [A criação da PVC falha devido a credenciais erradas ou acesso negado](#cred_failure) para verificar suas credenciais. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>Você especificou `ibm.io/auto-create-bucket: true` para criar automaticamente um depósito no {{site.data.keyword.cos_full_notm}}, mas as credenciais fornecidas no segredo do Kubernetes estão designadas à função de acesso ao serviço **Leitor** do IAM. Essa função não permite criação de depósito no {{site.data.keyword.cos_full_notm}}. </td>
          <td>Consulte [A criação do PVC falha devido a credenciais erradas ou a acesso negado](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>Você especificou `ibm.io/auto-create-bucket: true` e forneceu um nome de um depósito existente em `ibm.io/bucket`. Além disso, as credenciais que você forneceu no segredo do Kubernetes estão designadas à função de acesso ao serviço **Leitor** do IAM. Essa função não permite criação de depósito no {{site.data.keyword.cos_full_notm}}. </td>
          <td>Para usar um depósito existente, configure `ibm.io/auto-create-bucket: false` e forneça o nome de seu depósito existente em `ibm.io/bucket`. Para criar automaticamente um depósito usando seu segredo do Kubernetes existente, configure `ibm.io/bucket: ""` e siga [A criação da PVC falha devido a credenciais erradas ou acesso negado](#cred_failure) para verificar as credenciais em seu segredo do Kubernetes.</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>A chave de acesso secreto do {{site.data.keyword.cos_full_notm}} de suas credenciais do HMAC que você forneceu em seu segredo do Kubernetes não está correta. </td>
          <td>Consulte [A criação do PVC falha devido a credenciais erradas ou a acesso negado](#cred_failure).</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>O ID de chave de acesso do {{site.data.keyword.cos_full_notm}} ou a chave de acesso secreto de suas credenciais do HMAC que você forneceu em seu segredo do Kubernetes não estão corretos. </td>
          <td>Consulte [A criação do PVC falha devido a credenciais erradas ou a acesso negado](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>A chave de API do {{site.data.keyword.cos_full_notm}} de suas credenciais do IAM e o GUID de sua instância de serviço do {{site.data.keyword.cos_full_notm}} não estão corretos.</td>
          <td>Consulte [A criação do PVC falha devido a credenciais erradas ou a acesso negado](#cred_failure). </td>
        </tr>
  </tbody>
    </table>
    

### Armazenamento de objeto: a criação de PVC ou pod falha porque o segredo do Kubernetes não é localizado
{: #cos_secret_access_fails}

{: tsSymptoms}
Quando você cria seu PVC ou implementa um pod que monta o PVC, a criação ou a implementação falha.

- Mensagem de erro de exemplo para uma falha de criação de PVC:
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Mensagem de erro de exemplo para uma falha de criação do pod:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
O segredo do Kubernetes no qual você armazena suas credenciais de serviço do {{site.data.keyword.cos_full_notm}}, o PVC e o pod não estão todos no mesmo namespace do Kubernetes. Quando o segredo é implementado em um namespace diferente de seu PVC ou pod, o segredo não pode ser acessado.

{: tsResolve}
Essa tarefa requer a [função de serviço **Gravador** ou **Gerenciador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para todos os namespaces.

1. Liste os segredos em seu cluster e revise o namespace do Kubernetes no qual o segredo do Kubernetes para sua instância de serviço do {{site.data.keyword.cos_full_notm}} é criado. O segredo deve mostrar `ibm/ibmc-s3fs` como o **Tipo**.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. Verifique seu arquivo de configuração YAML quanto a seu PVC e pod para verificar se você usou o mesmo namespace. Se você desejar implementar um pod em um namespace diferente daquele em que seu segredo existe, [crie outro segredo](/docs/containers?topic=containers-object_storage#create_cos_secret) nesse namespace.

3. Crie o PVC ou implemente o pod no namespace apropriado.

<br />


### Armazenamento de objeto: a criação de PVC falha devido a credenciais erradas ou acesso negado
{: #cred_failure}

{: tsSymptoms}
Ao criar o PVC, você vê uma mensagem de erro semelhante a uma das seguintes:

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: falha ao carregar credenciais
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
As credenciais de serviço do {{site.data.keyword.cos_full_notm}} que você usa para acessar a instância de serviço podem estar erradas ou permitir acesso somente leitura para seu bucket.

{: tsResolve}
1. Na navegação na página de detalhes do serviço, clique em **Credenciais de serviço**.
2. Localize suas credenciais e, em seguida, clique em **Visualizar credenciais**.
3. Na seção **iam_role_crn**, verifique se você tem a função `Writer` ou `Manager`. Se você não tem a função correta, deve-se criar novas credenciais de serviço do {{site.data.keyword.cos_full_notm}} com a permissão correta. 
4. Se a função estiver correta, verifique se você usa o **access_key_id** e o **secret_access_key** corretos em seu segredo do Kubernetes. 
5. [Crie um novo segredo com o **access_key_id** e o **secret_access_key**](/docs/containers?topic=containers-object_storage#create_cos_secret) atualizados. 


<br />


### Armazenamento de objeto: a criação da PVC falha devido ao terminal de API do s3fs ou IAM errado
{: #cos_api_endpoint_failure}

{: tsSymptoms}
Quando você cria a PVC, a PVC permanece em um estado pendente. Depois de executar o comando `kubectl describe pvc <pvc_name>`, você verá uma das mensagens de erro a seguir: 

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
O terminal de API do s3fs para o depósito que você deseja usar pode ter o formato errado ou seu cluster está implementado em um local que é suportado no {{site.data.keyword.containerlong_notm}}, mas ainda não é suportado pelo plug-in do {{site.data.keyword.cos_full_notm}}. 

{: tsResolve}
1. Verifique o terminal de API do s3fs que foi designado automaticamente pelo plug-in do Helm `ibmc` para suas classes de armazenamento durante a instalação do plug-in do {{site.data.keyword.cos_full_notm}}. O terminal é baseado no local no qual seu cluster está implementado.  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   Se o comando retornar `ibm.io/object-store-endpoint: NA`, seu cluster será implementado em um local que é suportado no {{site.data.keyword.containerlong_notm}}, mas ainda não é suportado pelo plug-in do {{site.data.keyword.cos_full_notm}}. Para incluir o local no {{site.data.keyword.containerlong_notm}}, poste uma pergunta em nosso Slack público ou abra um caso de suporte do {{site.data.keyword.Bluemix_notm}}. Para obter mais informações, consulte [Obtendo ajuda e suporte](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). 
   
2. Se você incluiu manualmente o terminal de API do s3fs com a anotação `ibm.io/endpoint` ou o terminal de API do IAM com a anotação `ibm.io/iam-endpoint` em sua PVC, certifique-se de que tenha incluído os terminais no formato `https://<s3fs_api_endpoint>` e `https://<iam_api_endpoint>`. A anotação sobrescreve os terminais de API que são configurados automaticamente pelo plug-in `ibmc` nas classes de armazenamento do {{site.data.keyword.cos_full_notm}}. 
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}
   
<br />


### Armazenamento de objeto: não é possível acessar um bucket existente
{: #cos_access_bucket_fails}

{: tsSymptoms}
Quando você cria o PVC, o bucket no {{site.data.keyword.cos_full_notm}} não pode ser acessado. Você vê uma mensagem de erro semelhante à seguinte:

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
Você pode ter usado a classe de armazenamento errada para acessar seu depósito existente ou tentado acessar um depósito que você não criou. Não é possível acessar um depósito que você não criou. 

{: tsResolve}
1. No [painel do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/), selecione sua instância de serviço do {{site.data.keyword.cos_full_notm}}.
2. Selecione  ** Buckets **.
3. Revise as informações de **Classe** e **Localização** para o seu bucket existente.
4. Escolha a  [ classe de armazenamento ](/docs/containers?topic=containers-object_storage#cos_storageclass_reference) apropriada.
5. Certifique-se de configurar `ibm.io/auto-create-bucket: false` e fornecer o nome correto de seu depósito existente. 

<br />


## Armazenamento de objeto: falha ao acessar arquivos com um usuário não raiz
{: #cos_nonroot_access}

{: tsSymptoms}
Você transferiu por upload os arquivos para a sua instância de serviço do {{site.data.keyword.cos_full_notm}} usando o console ou a API de REST. Quando você tenta acessar esses arquivos com um usuário não raiz definido com `runAsUser` em sua implementação de app, o acesso aos arquivos é negado.

{: tsCauses}
No Linux, um arquivo ou um diretório tem 3 grupos de acesso: `Owner`, `Group` e `Other`. Ao fazer upload de um arquivo para o {{site.data.keyword.cos_full_notm}} usando o console ou a API de REST, as permissões para o `Owner`, `Group` e `Other` serão removidas. A permissão de cada arquivo é semelhante à seguinte:

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

Ao fazer upload de um arquivo usando o plug-in {{site.data.keyword.cos_full_notm}}, as permissões para o arquivo são preservadas e não mudam.

{: tsResolve}
Para acessar o arquivo com um usuário não raiz, o usuário não raiz deve ter permissões de leitura e gravação para o arquivo. Mudar a permissão em um arquivo como parte de sua implementação do pod requer uma operação de gravação. O {{site.data.keyword.cos_full_notm}}  não foi projetado para cargas de trabalho de gravação. A atualização de permissões durante a implementação do pod pode evitar que seu pod entre em um estado `Running`.

Para resolver esse problema, antes de montar o PVC em seu pod de app, crie outro pod para configurar a permissão correta para o usuário não raiz.

1. Verifique as permissões de seus arquivos em seu bucket.
   1. Crie um arquivo de configuração para seu pod `test-permission` e nomeie o arquivo como `test-permission.yaml`.
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. Crie o pod  ` test-permission ` .
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Efetue login em seu pod.
      ```
      kubectl exec test-permission-permission -it bash
      ```
      {: pre}

   4. Navegue para o caminho de montagem e liste as permissões para seus arquivos.
      ```
      cd test & & ls -al
      ```
      {: pre}

      Saída de exemplo:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. Exclua o pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Crie um arquivo de configuração para o pod que você usar para corrigir as permissões de seus arquivos e nomeie-o como `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission 
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. Crie o pod  ` fix-permission ` .
   ```
   kubectl aplicar -f fix-permission.yaml
   ```
   {: pre}

4. Espere o pod entrar em um estado `Completed`.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Exclua o pod `fix-permission`.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Recrie o pod `test-permission` que você usou anteriormente para verificar as permissões.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Verifique se as permissões para seus arquivos estão atualizadas.
   1. Efetue login em seu pod.
      ```
      kubectl exec test-permission-permission -it bash
      ```
      {: pre}

   2. Navegue para o caminho de montagem e liste as permissões para seus arquivos.
      ```
      cd test & & ls -al
      ```
      {: pre}

      Saída de exemplo:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. Exclua o pod  ` test-permission ` .
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Monte o PVC para o app com o usuário não raiz.

   Defina o usuário não raiz como `runAsUser` sem configurar `fsGroup` em sua implementação YAML ao mesmo tempo. Configurar o `fsGroup` aciona o plug-in do {{site.data.keyword.cos_full_notm}} para atualizar as permissões de grupo para todos os arquivos em um depósito quando o pod é implementado. A atualização das permissões é uma operação de gravação e pode evitar que seu pod entre em um estado `Running`.
   {: important}

Depois de configurar as permissões de arquivo corretas em sua instância de serviço do {{site.data.keyword.cos_full_notm}}, não faça upload dos arquivos usando o console ou a API de REST. Use o plug-in do {{site.data.keyword.cos_full_notm}} para incluir arquivos em sua instância de serviço.
{: tip}

<br />


   
## A criação da PVC falha devido a permissões ausentes
{: #missing_permissions}

{: tsSymptoms}
Quando você cria uma PVC, a PVC permanece pendente. Ao executar `kubectl describe pvc <pvc_name>`, você vê uma mensagem de erro semelhante à seguinte: 

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
A chave de API do IAM ou a chave de API de infraestrutura do IBM Cloud (SoftLayer) que está armazenada no segredo do Kubernetes `storage-secret-store` de seu cluster não tem todas as permissões necessárias para provisionar armazenamento persistente. 

{: tsResolve}
1. Recupere a chave do IAM ou a chave da API da infraestrutura do IBM Cloud (SoftLayer) que está armazenada no segredo do Kubernetes `storage-secret-store` do seu cluster e verifique se a chave da API correta é usada. 
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}
   
   Saída de exemplo: 
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}
   
   A chave de API do IAM é listada na seção `Bluemix.iam_api_key` de sua saída da CLI. Se o `Softlayer.softlayer_api_key` estiver vazio ao mesmo tempo, a chave de API do IAM será usada para determinar suas permissões de infraestrutura. A chave de API do IAM é configurada automaticamente pelo usuário que executa a primeira ação que requer a função da plataforma **Administrador** do IAM em um grupo de recursos e região. Se uma chave de API diferente for configurada em `Softlayer.softlayer_api_key`, essa chave terá precedência sobre a chave de API do IAM. O `Softlayer.softlayer_api_key` é configurado quando um administrador de cluster executa o comando `ibmcloud ks credentials credentials-set`. 
   
2. Se desejar mudar as credenciais, atualize a chave de API que é usada. 
    1.  Para atualizar a chave de API do IAM, use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset) `ibmcloud ks api-key-reset`. Para atualizar a chave de infraestrutura do IBM Cloud (SoftLayer), use o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set) `ibmcloud ks credential-set`.
    2. Aguarde cerca de 10 a 15 minutos para que o segredo do Kubernetes `storage-secret-store` seja atualizado e, em seguida, verifique se a chave foi atualizada.
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}
   
3. Se a chave de API estiver correta, verifique se a chave tem a permissão correta para provisionar armazenamento persistente.
   1. Entre em contato com o proprietário da conta para verificar a permissão da chave de API. 
   2. Como proprietário da conta, selecione **Gerenciar** > **Acesso (IAM)** por meio da navegação no console do {{site.data.keyword.Bluemix_notm}}.
   3. Selecione **Usuários** e localize o usuário cuja chave de API você deseja usar. 
   4. No menu de ações, selecione **Gerenciar detalhes do usuário**. 
   5. Acesse a guia **Infraestrutura clássica**. 
   6. Expanda a categoria **Conta** e verifique se a permissão **Incluir/Fazer upgrade do armazenamento (StorageLayer)** está designada. 
   7. Expanda a categoria **Serviços** e verifique se a permissão **Gerenciar armazenamento** está designada. 
4. Remova a PVC que falhou. 
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}
   
5. Recrie a PVC. 
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## Obtendo ajuda e suporte
{: #storage_getting_help}

Ainda está tendo problemas com o seu cluster?
{: shortdesc}

-  No terminal, você é notificado quando atualizações para a CLI `ibmcloud` e plug-ins estão disponíveis. Certifique-se de manter sua CLI atualizada para que seja possível usar todos os comandos e sinalizações disponíveis.
-   Para ver se o {{site.data.keyword.Bluemix_notm}} está disponível, [verifique a página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/status?selected=status).
-   Poste uma pergunta no [{{site.data.keyword.containerlong_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com).
    Se você não estiver usando um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para essa Folga.
    {: tip}
-   Revise os fóruns para ver se outros usuários tiveram o mesmo problema. Ao usar os fóruns para fazer uma pergunta, marque sua pergunta para que ela seja vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.
    -   Se você tiver questões técnicas sobre como desenvolver ou implementar clusters ou apps com o {{site.data.keyword.containerlong_notm}}, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") ](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e identifique-a com `ibm-cloud`, `kubernetes` e `containers`.
    -   Para perguntas sobre o serviço e instruções de introdução, use o fórum do [IBM Developer Answers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Inclua as tags `ibm-cloud` e `containers`.
    Consulte
[Obtendo
ajuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obter mais detalhes sobre o uso dos fóruns.
-   Entre em contato com o Suporte IBM abrindo um caso. Para saber mais sobre como abrir um caso de suporte IBM ou sobre os níveis de suporte e as severidades do caso, consulte [Entrando em contato com o suporte](/docs/get-support?topic=get-support-getting-customer-support).
Ao relatar um problema, inclua o ID do cluster. Para obter o ID do seu cluster, execute `ibmcloud ks clusters`. É possível também usar o [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para reunir e exportar informações pertinentes de seu cluster para compartilhar com o Suporte IBM.
{: tip}

