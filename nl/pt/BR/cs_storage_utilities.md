---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, local persistent storage

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



# Utilitários de armazenamento do IBM
{: #utilities}

## Instalando o plug-in do IBM Cloud Block Storage Attacher (beta)
{: #block_storage_attacher}

Use o {{site.data.keyword.Bluemix_notm}} Plug-in do Attacher de armazenamento de bloco para anexar armazenamento de bloco bruto, não formatado e desmontado em um nó do trabalhador em seu cluster.  
{: shortdesc}

Por exemplo, você deseja armazenar seus dados com uma solução de armazenamento definida pelo software (SDS), como [Portworx](/docs/containers?topic=containers-portworx), mas você não deseja usar nós do trabalhador bare metal que são otimizados para uso do SDS e que vêm com discos locais extras. Para incluir discos locais em seu nó do trabalhador não SDS, deve-se criar manualmente seus dispositivos de armazenamento de bloco em sua conta de infraestrutura do {{site.data.keyword.Bluemix_notm}} e usar o {{site.data.keyword.Bluemix_notm}} Block Volume Attacher para anexar o armazenamento ao nó do trabalhador não SDS.

O plug-in do {{site.data.keyword.Bluemix_notm}} Block Volume Attacher cria os pods em cada nó do trabalhador em seu cluster como parte de um conjunto de daemons e configura uma classe de armazenamento do Kubernetes que você usa posteriormente para conectar o dispositivo de armazenamento de bloco ao nó do trabalhador não SDS.

Procurando instruções sobre como atualizar ou remover o plug-in do {{site.data.keyword.Bluemix_notm}} Block Volume Attacher? Consulte [Atualizando o plug-in](#update_block_attacher) e [Removendo o plug-in](#remove_block_attacher).
{: tip}

1.  [Siga as instruções](/docs/containers?topic=containers-helm#public_helm_install) para instalar o cliente Helm em sua máquina local e instale o servidor Helm (tiller) com uma conta do serviço em seu cluster.

2.  Verifique se o tiller está instalado com uma conta do serviço.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Saída de exemplo:

    ```
    NAME SECRETS AGE tiller 1 2m
    ```
    {: screen}

3. Atualize o repositório Helm para recuperar a versão mais recente de todos os gráficos Helm nesse repositório.
   ```
   helm repo update
   ```
   {: pre}

4. Instale o  {{site.data.keyword.Bluemix_notm}}  Plug-in do Attacher de volume de bloco do  {{site.data.keyword.Bluemix_notm}} . Quando você instala o plug-in, classes de armazenamento de bloco predefinidas são incluídas no cluster.
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   Saída de exemplo:
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   == >v1/StorageClass
   NOME DO PROVISOR DE NOME
   ibmc-block-attacher ibm.io/ibmc-blockattacher 1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTAS:
   Agradecemos a instalação de: ibmcloud-block-storage-attacher.   Sua liberação é nomeada: block-volume-attacher

   Consulte o arquivo README.md do gráfico para anexar um armazenamento de bloco
   Consulte o RELEASE.md do gráfico para ver os detalhes/correções da liberação
   ```
   {: screen}

5. Verifique se o {{site.data.keyword.Bluemix_notm}} Conjunto de Daemon do Attacher do Volume do Bloqueio foi instalado com êxito.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Saída de exemplo:
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   A instalação é bem-sucedida quando você vê um ou mais pods **ibmcloud-block-storage-attacher**. O número de pods é igual ao número de nós do trabalhador em seu cluster. Todos os pods devem estar em um estado **Executando**.

6. Verifique se a classe de armazenamento para o {{site.data.keyword.Bluemix_notm}} Block Volume Attacher foi criada com êxito.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Saída de exemplo:
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### Atualizando o plug-in do IBM Cloud Block Storage Attacher
{: #update_block_attacher}

É possível fazer upgrade do plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage Attacher existente para a versão mais recente.
{: shortdesc}

1. Atualize o repositório Helm para recuperar a versão mais recente de todos os gráficos Helm nesse repositório.
   ```
   helm repo update
   ```
   {: pre}

2. Opcional: faça download do gráfico Helm mais recente em sua máquina local. Em seguida, extraia o pacote e revise o arquivo `release.md` para localizar as informações de liberação mais recentes.
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. Localize o nome do gráfico do Helm para o plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Saída de exemplo:
   ```
   <helm_chart_name>	1 	Wed Aug 1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. Atualize o {{site.data.keyword.Bluemix_notm}} Block Storage Attacher para o mais recente.
   ```
   upgrade do leme -- force -- recreate-pods < helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### Removendo o plug-in do IBM Cloud Block Volume Attacher
{: #remove_block_attacher}

Se você não desejar fornecer e usar o plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage Attacher em seu cluster, será possível desinstalar o gráfico do Helm.
{: shortdesc}

1. Localize o nome do gráfico do Helm para o plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage Attacher.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Saída de exemplo:
   ```
   <helm_chart_name>	1 	Wed Aug 1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Exclua o plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage Attacher removendo o gráfico do Helm.
   ```
   helm delete < helm_chart_name> -- purge
   ```
   {: pre}

3. Verifique se os pods do plug-in do {{site.data.keyword.Bluemix_notm}} Block Storage Attacher são removidos.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   A remoção dos pods será bem-sucedida se nenhum pod for exibido na saída da CLI.

4. Verifique se a classe de armazenamento do {{site.data.keyword.Bluemix_notm}} Block Storage Attacher foi removida.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   A remoção da classe de armazenamento será bem-sucedida se nenhuma classe de armazenamento for exibida na saída da CLI.

## Fornecendo automaticamente o armazenamento de bloco não formatado e autorizando seus nós do trabalhador a acessarem o armazenamento
{: #automatic_block}

É possível usar o plug-in do {{site.data.keyword.Bluemix_notm}} Block Volume Attacher para incluir automaticamente o armazenamento de bloco bruto, não formatado e desmontado com a mesma configuração para todos os nós do trabalhador em seu cluster.
{: shortdesc}

O contêiner `mkpvyaml` que está incluído no plug-in {{site.data.keyword.Bluemix_notm}} Block Volume Attacher é configurado para executar um script que localiza todos os nós do trabalhador em seu cluster, cria armazenamento de bloco bruto no {{site.data.keyword.Bluemix_notm}} portal de infra-estrutura e, em seguida, autoriza os nós do trabalhador a acessam o armazenamento.

Para incluir configurações de armazenamento de bloco diferentes, inclua o armazenamento de bloco em um subconjunto de nós do trabalhador apenas ou para ter mais controle sobre o processo de fornecimento, escolha [incluir manualmente o armazenamento de bloco](#manual_block).
{: tip}


1. Efetue login no {{site.data.keyword.Bluemix_notm}} e tenha como destino o grupo de recursos em que seu cluster está.
   ```
   ibmcloud login
   ```
   {: pre}

2.  Clone o repo do  {{site.data.keyword.Bluemix_notm}}  Storage Utilities.
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. Navegue para o diretório  ` block-storage-utilities ` .
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. Abra o arquivo `yamlgen.yaml` e especifique a configuração de armazenamento de bloco que você deseja incluir em cada nó do trabalhador no cluster.
   ```
   #
   # É possível especificar somente 'performance' OU 'endurance' e a cláusula associada
   #
   cluster:  <cluster_name>    #  Insira o nome do seu cluster
   region:   <region>          #  Insira a região do IBM Cloud Kubernetes Service a qual criou seu cluster
   type:  <storage_type>       #  Insira o tipo de armazenamento que deseja fornecer. Escolha entre a oferta "performance" ou "endurance": storage_as_a_service
   # performance:
   #    - iops:  <iops>
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Insira o tamanho do seu armazenamento em gigabytes
   ```
   {: codeblock}

   <table>
   <caption>Entendendo os componentes de arquivo YAML</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Ícone de ideia"/> entendendo os componentes de arquivo do YAML</th>
   </thead>
   <tbody>
   <tr>
   <td><code> cluster </code></td>
   <td>Insira o nome de seu cluster no qual deseja incluir o armazenamento de bloco bruto. </td>
   </tr>
   <tr>
   <td><code>região</code></td>
   <td>Insira a {{site.data.keyword.containerlong_notm}} região na qual você criou seu cluster. Execute <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code> para localizar a região do seu cluster.  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>Insira o tipo de armazenamento que você deseja fornecer. Escolha entre  <code> desempenho </code>  ou  <code> endurance </code>. Para obter mais informações, consulte [Decidindo sobre sua configuração de armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).  </td>
   </tr>
   <tr>
   <td><code>desempenho.iops</code></td>
   <td>Se desejar fornecer o armazenamento de `performance`, insira o número de IOPS. Para obter mais informações, consulte [Decidindo sobre sua configuração de armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Se desejar fornecer o armazenamento de `endurance`, remova esta seção ou comente-a incluindo `#` no início de cada linha.
   </tr>
   <tr>
   <td><code> endurance.tier </code></td>
   <td>Se desejar fornecer o armazenamento de `endurance`, insira o número de IOPS por gigabyte. Por exemplo, se você desejar fornecer o armazenamento de bloco como ele está definido na classe de armazenamento `ibmc-block-bronze`, insira 2. Para obter mais informações, consulte [Decidindo sobre a configuração de armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). Se desejar fornecer o armazenamento de `performance`, remova esta seção ou comente-a incluindo `#` no início de cada linha. </td>
   </tr>
   <tr>
   <td><code>Padrão</code></td>
   <td>Insira o tamanho de seu armazenamento em gigabytes. Consulte [Decidindo sobre a configuração de armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para localizar tamanhos suportados para a sua camada de armazenamento. </td>
   </tr>
   </tbody>
   </table>  

5. Recupere o nome do usuário da infraestrutura do IBM Cloud (SoftLayer) e a chave de API. O nome do usuário e a chave API são usados pelo script `mkpvvyaml` para acessar o cluster.
   1. Efetue login no [console do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/).
   2. No menu ![Ícone de menu](../icons/icon_hamburger.svg "Ícone de menu"), selecione **Infraestrutura**.
   3. Na barra de menus, selecione **Conta** > **Usuários** > **Lista de usuários**.
   4. Localize o usuário cujo nome de usuário e chave de API você deseja recuperar.
   5. Clique em **Gerar** para gerar uma chave de API ou **Visualizar** para visualizar sua chave de API existente. Uma janela pop-up é aberta que mostra o nome do usuário e a chave de API da infraestrutura.

6. Armazene as credenciais em uma variável de ambiente.
   1. Inclua as variáveis de ambiente.
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. Verifique se as variáveis de ambiente foram criadas com êxito.
      ```
      printenv
      ```
      {: pre}

7.  Construa e execute o contêiner  ` mkpvyaml ` . Quando você executa o contêiner por meio da imagem, o script `mkpvyaml.py` é executado. O script inclui um dispositivo de armazenamento de bloco em cada nó do trabalhador no cluster e autoriza cada nó do trabalhador a acessar o dispositivo de armazenamento de bloco. No final do script, um arquivo IAML `pv-<cluster_name>.yaml` é gerado e pode ser usado posteriormente para criar os volumes persistentes no cluster.
    1.  Construa o contêiner  ` mkpvyaml ` .
        ```
        docker build -t mkpvyaml.
        ```
        {: pre}
        Saída de exemplo:
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  Execute o contêiner para executar o script `mkpvyaml.py`.
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        Saída de exemplo:
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [Conecte os dispositivos de armazenamento de bloco a seus nós do trabalhador](#attach_block).

## Incluindo manualmente o armazenamento de bloco em nós do trabalhador específicos
{: #manual_block}

Use essa opção se você desejar incluir configurações de armazenamento de bloco diferentes, incluir armazenamento de bloco em um subconjunto de nós do trabalhador apenas ou ter mais controle sobre o processo de fornecimento.
{: shortdesc}

1. Liste os nós do trabalhador em seu cluster e anote o endereço IP privado e a zona dos nós do trabalhador não SDS nos quais deseja incluir um dispositivo de armazenamento de bloco.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. Revise a etapa 3 e 4 em [Decidindo sobre a configuração de armazenamento de bloco](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) para escolher o tipo, o tamanho e o número de IOPS para o dispositivo de armazenamento de bloco que você deseja incluir em seu nó do trabalhador não SDS.    

3. Crie o dispositivo de armazenamento de bloco na mesma zona em que seu nó do trabalhador não SDS está.

   **Exemplo para provisionar 20 GB de armazenamento de bloco do Endurance com dois IOPS por GB:**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **Exemplo para provisionar o armazenamento de bloco de desempenho de 20 GB com 100 IOPS:**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. Verifique se o dispositivo de armazenamento de bloco foi criado e anote o **`id`** do volume. **Nota:** se você não vir seu dispositivo de armazenamento de bloco imediatamente, aguarde alguns minutos. Em seguida, execute este comando novamente.
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   Saída de exemplo:
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. Revise os detalhes para seu volume e anote o **`Target IP`** e o **`LUN Id`**.
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   Saída de exemplo:
   ```
   Name                       Value
   ID                         1234567890
   User name                  IBM123A4567890-1
   Type                       performance_block_storage
   Capacity (GB)              20
   LUN Id                     0
   IOPs                       100
   Datacenter                 dal10
   Target IP                  161.12.34.123
   # of Active Transactions   0
   Replicant Count            0
   ```
   {: screen}

6. Autorize o nó do trabalhador não SDS a acessar o dispositivo de armazenamento de bloco. Substitua `<volume_ID>` pelo ID de volume do seu dispositivo de armazenamento de bloco recuperado anteriormente e `<private_worker_IP>` pelo endereço IP privado do nó do trabalhador que não é SDS no qual deseja conectar o dispositivo.

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   Saída de exemplo:
   ```
   O endereço IP 123456789 foi autorizado a acessar <volume_ID>.
   ```
   {: screen}

7. Verifique se o nó do trabalhador não SDS está autorizado com êxito e anote o **`host_iqn`**, o **`username`** e a **`password`**.
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   Saída de exemplo:
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   A autorização é bem-sucedida quando o **`host_iqn`**, o **`username`** e a **`password`** são designados.

8. [Conecte os dispositivos de armazenamento de bloco a seus nós do trabalhador](#attach_block).


## Anexando o armazenamento de bloco bruto em nós do trabalhador não SDS
{: #attach_block}

Para conectar o dispositivo de armazenamento de bloco a um nó do trabalhador não SDS, deve-se criar um volume persistente (PV) com a classe de armazenamento do {{site.data.keyword.Bluemix_notm}} Block Volume Attacher e os detalhes de seu dispositivo de armazenamento de bloco.
{: shortdesc}

** Antes de iniciar **:
- Certifique-se de que você tenha criado [automaticamente](#automatic_block) ou [manualmente](#manual_block) o armazenamento de bloco bruto, não formatado e desmontado em seus nós do trabalhador não SDS.
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**Para conectar o armazenamento de bloco bruto a nós do trabalhador não SDS**:
1. Prepare a criação do PV.  
   - **Se você usou o contêiner `mkpvyaml` container:**
     1. Abra o arquivo `pv-<cluster_name>.yaml`.
        ```
        nano pv-< cluster_name> .yaml
        ```
        {: pre}

     2. Revise a configuração para seus PVs.

   - ** Se você incluiu manualmente o armazenamento de bloco: **
     1. Crie um arquivo  ` pv.yaml ` . O comando a seguir cria o arquivo com o editor `nano`.
        ```
        nano pv.yaml
        ```
        {: pre}

     2. Inclua os detalhes de seu dispositivo de armazenamento de bloco para o PV.
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
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
      	<td>Insira um nome para seu PV.</td>
      	</tr>
        <tr>
        <td><code> ibm.io/iqn </code></td>
        <td>Insira o nome do host IQN que você recuperou anteriormente. </td>
        </tr>
        <tr>
        <td><code> ibm.io/username </code></td>
        <td>Insira o nome do usuário da infraestrutura do IBM Cloud (SoftLayer) que você recuperou anteriormente. </td>
        </tr>
        <tr>
        <td><code>ibm.io/senha</code></td>
        <td>Insira a senha de infraestrutura do IBM Cloud (SoftLayer) que você recuperou anteriormente. </td>
        </tr>
        <tr>
        <td><code> ibm.io/targetip </code></td>
        <td>Insira o IP de destino que você recuperou anteriormente. </td>
        </tr>
        <tr>
        <td><code> ibm.io/lunid </code></td>
        <td>Insira o ID de LUN do dispositivo de armazenamento de bloco que você recuperou anteriormente. </td>
        </tr>
        <tr>
        <td><code> ibm.io/nodeip </code></td>
        <td>Insira o endereço IP privado do nó do trabalhador no qual você deseja anexar o dispositivo de armazenamento de bloco e que você autorizou anteriormente para acessar seu dispositivo de armazenamento de bloco. </td>
        </tr>
        <tr>
          <td><code> ibm.io/volID </code></td>
        <td>Insira o ID do volume de armazenamento de bloco que você recuperou anteriormente. </td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>Insira o tamanho do dispositivo de armazenamento de bloco que você criou anteriormente. Por exemplo, se seu dispositivo de armazenamento de bloco tiver 20 gigabytes, insira <code>20Gi</code>.  </td>
        </tr>
        </tbody>
        </table>
2. Crie o PV para anexar o dispositivo de armazenamento de bloco ao nó do trabalhador não SDS.
   - ** Se você usou o contêiner `mkpvyaml`:**
     ```
     kubectl apply -f pv-< cluster_name> .yaml
     ```
     {: pre}

   - **Se você incluiu manualmente o armazenamento de bloco:**
     ```
     kubectl apply -f block-volume-attacher/pv.yaml
     ```
     {: pre}

3. Verifique se o armazenamento de bloco foi conectado com êxito ao nó do trabalhador.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Saída de exemplo:
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizadores: [ ]
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:
   Events:            <none>
   ```
   {: screen}

   O dispositivo de armazenamento de bloco é conectado com sucesso quando o **ibm.io/dm** é configurado para um ID de dispositivo, como `/dev/dm/1`, e é possível ver **ibm.io/attachstatus = conectado** na seção **Anotações** de sua saída da CLI.

Se desejar desconectar um volume, exclua o PV. Os volumes desconectados ainda estão autorizados a serem acessados por um nó do trabalhador específico e são conectados novamente quando você cria um novo PV com a classe de armazenamento do {{site.data.keyword.Bluemix_notm}} Block Volume Attacher para anexar um volume diferente ao mesmo nó do trabalhador. Para evitar a anexação do volume desconectado antigo novamente, remova a autorização do nó do trabalhador para acessar o volume desconectado usando o comando `ibmcloud sl block access-revoke`. A remoção do volume não remove o volume de sua conta de infraestrutura do IBM Cloud (SoftLayer). Para cancelar o faturamento para seu volume, deve-se [remover manualmente o armazenamento de sua conta de infraestrutura do IBM Cloud (SoftLayer)](/docs/containers?topic=containers-cleanup).
{: note}


