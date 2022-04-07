---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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



# Incluindo serviços com o uso de gráficos do Helm
{: #helm}

É possível incluir aplicativos complexos do Kubernetes em seu cluster usando gráficos do Helm.
{: shortdesc}

**O que é o Helm e como utilizá-lo?**</br>
[O Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://helm.sh) é um gerenciador de pacotes do Kubernetes que usa gráficos do Helm para definir, instalar e fazer upgrade de aplicativos complexos do Kubernetes em seu cluster. Os gráficos do Helm empacotam as especificações para gerar arquivos YAML para recursos do Kubernetes que constroem seu aplicativo. Esses recursos do Kubernetes são aplicados automaticamente a seu cluster e designados a uma versão pelo Helm. Também é possível usar o Helm para especificar e empacotar seu próprio aplicativo e permitir que o Helm gere os arquivos YAML para seus recursos do Kubernetes.  

Para usar o Helm no cluster, deve-se instalar a CLI do Helm na máquina local e o Tiller do servidor Helm em cada cluster no qual o Helm será usado.

**Quais gráficos do Helm são suportados no {{site.data.keyword.containerlong_notm}}?**</br>
Para obter uma visão geral dos gráficos do Helm disponíveis, consulte o [Catálogo de gráficos do Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts). Os gráficos do Helm que estão listados neste catálogo são agrupados conforme a seguir:

- **iks-charts**: gráficos do Helm aprovados para o {{site.data.keyword.containerlong_notm}}. O nome desse repositório foi mudado de `ibm` para `iks-charts`.
- **ibm-charts**: gráficos do Helm aprovados para clusters do {{site.data.keyword.containerlong_notm}} e do {{site.data.keyword.Bluemix_notm}} Private.
- **kubernetes**: gráficos do Helm fornecidos pela comunidade do Kubernetes e considerados `stable` pelo controle da comunidade. Esses gráficos não são verificados para trabalhar no {{site.data.keyword.containerlong_notm}} ou em clusters do {{site.data.keyword.Bluemix_notm}} Private.
- **kubernetes-incubator**: gráficos do Helm fornecidos pela comunidade do Kubernetes e considerados `incubator` pelo controle da comunidade. Esses gráficos não são verificados para trabalhar no {{site.data.keyword.containerlong_notm}} ou em clusters do {{site.data.keyword.Bluemix_notm}} Private.

Os gráficos do Helm dos repositórios **iks-charts** e **ibm-charts** são totalmente integrados na organização de suporte do {{site.data.keyword.Bluemix_notm}}. Se tiver uma pergunta ou um problema com o uso desses gráficos do Helm, será possível usar um dos canais de suporte do {{site.data.keyword.containerlong_notm}}. Para obter mais informações, consulte [Obtendo ajuda e suporte](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

**Quais são os pré-requisitos para usar o Helm e posso usá-lo em um cluster privado?**</br>
Para implementar os gráficos do Helm, deve-se instalar a CLI do Helm em sua máquina local e instalar o Tiller do servidor Helm em seu cluster. A imagem para o Tiller é armazenada no Google Container Registry público. Para acessar a imagem durante a instalação do Tiller, seu cluster deve permitir a conectividade de rede pública para o Google Container Registry público. Os clusters que ativam o terminal em serviço público podem acessar automaticamente a imagem. Os clusters privados que são protegidos com um firewall customizado ou os clusters que ativaram somente o terminal em serviço privado não permitem acesso à imagem do Tiller. Em vez disso, é possível [extrair a imagem para a sua máquina local e enviar por push a imagem para o seu namespace no {{site.data.keyword.registryshort_notm}}](#private_local_tiller) ou [instalar os gráficos do Helm sem usar o Tiller](#private_install_without_tiller).


## Configurando o Helm em um cluster com acesso público
{: #public_helm_install}

Se o seu cluster ativou o terminal em serviço público, é possível instalar o servidor do Helm, o Tiller, usando a imagem pública no Google Container Registry.
{: shortdesc}

Antes de iniciar:
- [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Para instalar o Tiller com uma conta de serviço do Kubernetes e a ligação de função de cluster no espaço de nomes `kube-system`, certifique-se de ter a função [`cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Para instalar o Helm em um cluster com acesso público:

1. Instale a <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> em sua máquina local.

2. Verifique se você já instalou o Tiller com uma conta de serviço do Kubernetes em seu cluster.
   ```
   kubectl get serviceaccount --all-namespaces | grep tiller
   ```
   {: pre}

   Saída de exemplo se o Tiller estiver instalado:
   ```
   kube-system      tiller                               1         189d
   ```
   {: screen}

   A saída de exemplo inclui o namespace do Kubernetes e o nome da conta de serviço para o Tiller. Se o Tiller não estiver instalado com uma conta de serviço em seu cluster, nenhuma saída será retornada da CLI.

3. **Importante**: para manter a segurança do cluster, configure o Tiller com uma conta de serviço e ligação de função de cluster em seu cluster.
   - **Se o Tiller estiver instalado com uma conta de serviço:**
     1. Crie uma ligação de função de cluster para a conta de serviço do Tiller. Substitua `<namespace>` pelo namespace de instalação do Tiller em seu cluster.
        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=<namespace>:tiller -n <namespace>
        ```
        {: pre}

     2. Atualize o Tiller. Substitua `<tiller_service_account_name>` pelo nome da conta de serviço do Kubernetes para o Tiller recuperada na etapa anterior.
        ```
        helm init --upgrade --service-account <tiller_service_account_name>
        ```
        {: pre}

     3. Verifique se o pod `tiller-deploy` tem um **Status** de `Executando` em seu cluster.
        ```
        kubectl get pods -n <namespace> -l app=helm
        ```
        {: pre}

        Saída de exemplo:

        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

   - **Se o Tiller não estiver instalado com uma conta de serviço:**
     1. Crie uma conta de serviço do Kubernetes e uma ligação de função de cluster para o Tiller no namespace `kube-system` de seu cluster.
        ```
        kubectl create serviceaccount tiller -n kube-system
        ```
        {: pre}

        ```
        kubectl create clusterrolebinding tiller --clusterrole=cluster-admin --serviceaccount=kube-system:tiller -n kube-system
        ```
        {: pre}

     2. Verifique se a conta de serviço do Tiller foi criada.
        ```
        kubectl get serviceaccount -n kube-system tiller
        ```
        {: pre}

        Saída de exemplo:
        ```
        NAME SECRETS AGE tiller 1 2m
        ```
        {: screen}

     3. Inicialize a CLI do Helm e instale o Tiller em seu cluster com a conta de serviço criada.
        ```
        Helm init -- tiller de serviço da conta
        ```
        {: pre}

     4. Verifique se o pod `tiller-deploy` tem um **Status** de `Executando` em seu cluster.
        ```
        kubectl get pods -n kube-system -l app=helm
        ```
        {: pre}

        Saída de exemplo:
        ```
        NAME                            READY     STATUS    RESTARTS   AGE
    tiller-deploy-352283156-nzbcm   1/1       Running   0          2m
        ```
        {: screen}

4. Inclua os repositórios Helm do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

5. Atualize os repositórios para recuperar as versões mais recentes de todos os gráficos do Helm.
   ```
   helm repo update
   ```
   {: pre}

6. Liste os gráficos Helm que estão atualmente disponíveis nos repositórios do {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

7. Identifique o gráfico do Helm que você deseja instalar e siga as instruções no `README` do gráfico do Helm para instalar o gráfico do Helm em seu cluster.


## Clusters privados: enviando por push a imagem do Tiller para seu namespace no IBM Cloud Container Registry
{: #private_local_tiller}

É possível fazer pull da imagem do Tiller para sua máquina local, enviá-la por push para seu namespace no {{site.data.keyword.registryshort_notm}} e instalar o Tiller em seu cluster privado usando a imagem em {{site.data.keyword.registryshort_notm}}.
{: shortdesc}

Se desejar instalar um gráfico do Helm sem usar o Tiller, consulte [Clusters privados: instalando gráficos do Helm sem usar o Tiller](#private_install_without_tiller).
{: tip}

Antes de iniciar:
- Instale o Docker em sua máquina local. Se você instalou a [CLI do {{site.data.keyword.Bluemix_notm}}](/docs/cli?topic=cloud-cli-getting-started), o Docker já está instalado.
- [Instale o plug-in da CLI do {{site.data.keyword.registryshort_notm}} e configure um namespace](/docs/services/Registry?topic=registry-getting-started#gs_registry_cli_install).
- Para instalar o Tiller com uma conta de serviço do Kubernetes e a ligação de função de cluster no espaço de nomes `kube-system`, certifique-se de ter a função [`cluster-admin`](/docs/containers?topic=containers-users#access_policies).

Para instalar o Tiller usando o  {{site.data.keyword.registryshort_notm}}:

1. Instale a <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> em sua máquina local.
2. Conecte-se ao seu cluster privado usando o túnel VPN da infraestrutura do {{site.data.keyword.Bluemix_notm}} que você configurou.
3. **Importante**: para manter a segurança do cluster, crie uma conta de serviço para o Tiller no namespace `kube-system` e uma ligação de função de cluster RBAC do Kubernetes para o pod `tiller-deploy` aplicando o seguinte arquivo YAML do [repositório `kube-samples` do {{site.data.keyword.Bluemix_notm}}](https://github.com/IBM-Cloud/kube-samples/blob/master/rbac/serviceaccount-tiller.yaml).
    1. [Obtenha a conta de serviço do Kubernetes e o arquivo YAML de ligação de função de cluster ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/rbac/serviceaccount-tiller.yaml).

    2. Crie os recursos do Kubernetes em seu cluster.
       ```
       kubectl aplicar -f service-account.yaml
       ```
       {: pre}

       ```
       kubectl apply -f cluster-role-binding.yaml
       ```
       {: pre}

4. [Localize a versão do Tiller ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://console.cloud.google.com/gcr/images/kubernetes-helm/GLOBAL/tiller?gcrImageListsize=30) que deseja instalar em seu cluster. Se você não precisar de uma versão específica, use a mais recente.

5. Faça pull da imagem do Tiller do Google Container Registry público para sua máquina local.
   ```
   docker pull gcr.io/kubernetes-helm/tiller:v2.11.0
   ```
   {: pre}

   Saída de exemplo:
   ```
   docker, puxe gcr.io/kubernetes-helm/tiller:v2.13.0
   v2.13.0: Pulling a partir de kubernetes-helm/tiller
   48ecbb6b270e: Pull concluído
   d3fa0712c71b: Pull concluído
   bf13a43b92e9: Pull concluído
   b3f98be98675: Pull concluído
   Digest: sha256 :c4bf03bb67b3ae07e38e834f29dc7fd43f472f67cad3c078279ff1bbbbb463aa6
   Status: Imagem mais recente descarregada para gcr.io/kubernetes-helm/tiller:v2.13.0
   ```
   {: screen}

6. [Envie por push a imagem do Tiller para seu namespace no {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-getting-started#gs_registry_images_pushing).

7. Para acessar a imagem no {{site.data.keyword.registryshort_notm}} de dentro de seu cluster, [copie o segredo de pull de imagem do namespace padrão para o namespace `kube-system`](/docs/containers?topic=containers-images#copy_imagePullSecret).

8. Instale o Tiller em seu cluster privado usando a imagem que você armazenou em seu namespace no {{site.data.keyword.registryshort_notm}}.
   ```
   helm init --tiller-image <region>.icr.io/<mynamespace>/<myimage>:<tag> --service-account tiller
   ```
   {: pre}

9. Inclua os repositórios Helm do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

10. Atualize os repositórios para recuperar as versões mais recentes de todos os gráficos do Helm.
    ```
    helm repo update
    ```
    {: pre}

11. Liste os gráficos Helm que estão atualmente disponíveis nos repositórios do {{site.data.keyword.Bluemix_notm}}.
    ```
    helm search iks-charts
    ```
    {: pre}

    ```
    helm search ibm-charts
    ```
    {: pre}

12. Identifique o gráfico do Helm que você deseja instalar e siga as instruções no `README` do gráfico do Helm para instalar o gráfico do Helm em seu cluster.


## Clusters Privados: Instalando os Gráficos Helm sem Usar o Tiller
{: #private_install_without_tiller}

Se você não desejar instalar o Tiller em seu cluster privado, será possível criar manualmente os arquivos YAML do gráfico do Helm e aplicá-los usando os comandos `kubectl`.
{: shortdesc}

As etapas neste exemplo mostram como instalar os gráficos do Helm por meio dos repositórios do gráfico do Helm do {{site.data.keyword.Bluemix_notm}} em seu cluster privado. Se você deseja instalar um gráfico do Helm que não está armazenado em um dos repositórios do gráfico do Helm do {{site.data.keyword.Bluemix_notm}}, deve-se seguir as instruções neste tópico para criar os arquivos YAML para o seu gráfico do Helm. Além disso, deve-se fazer download da imagem do gráfico do Helm por meio do registro do contêiner público, enviá-la por push para seu namespace no {{site.data.keyword.registryshort_notm}} e atualizar o arquivo `values.yaml` para usar a imagem no {{site.data.keyword.registryshort_notm}}.
{: note}

1. Instale a <a href="https://docs.helm.sh/using_helm/#installing-helm" target="_blank">CLI do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> em sua máquina local.
2. Conecte-se ao seu cluster privado usando o túnel VPN da infraestrutura do {{site.data.keyword.Bluemix_notm}} que você configurou.
3. Inclua os repositórios Helm do {{site.data.keyword.Bluemix_notm}} em sua instância do Helm.
   ```
   helm repo add iks-charts https://icr.io/helm/iks-charts
   ```
   {: pre}

   ```
   helm repo add ibm-charts https://icr.io/helm/ibm-charts
   ```
   {: pre}

4. Atualize os repositórios para recuperar as versões mais recentes de todos os gráficos do Helm.
   ```
   helm repo update
   ```
   {: pre}

5. Liste os gráficos Helm que estão atualmente disponíveis nos repositórios do {{site.data.keyword.Bluemix_notm}}.
   ```
   helm search iks-charts
   ```
   {: pre}

   ```
   helm search ibm-charts
   ```
   {: pre}

6. Identifique o gráfico do Helm que deseja instalar, faça download dele em sua máquina local e descompacte os arquivos de seu gráfico do Helm. O exemplo a seguir mostra como fazer download do gráfico Helm para o cluster autoscaler versão 1.0.3 e descompactar os arquivos em um diretório `cluster-autoscaler`.
   ```
   helm fetch iks-charts/ibm-iks-cluster-autoscaler --untar --untardir ./cluster-autoscaler --version 1.0.3
   ```
   {: pre}

7. Navegue para o diretório no qual você descompactado os arquivos do gráfico do Helm.
   ```
   cd cluster-autoscaler
   ```
   {: pre}

8. Crie um diretório `output` para os arquivos YAML que você gerar usando os arquivos em seu gráfico do Helm.
   ```
   Saída mkdir
   ```
   {: pre}

9. Abra o arquivo `values.yaml` e faça qualquer mudança que seja requerida pelas instruções de instalação do gráfico do Helm.
   ```
   nano ibm-iks-cluster-autoscaler/values.yaml
   ```
   {: pre}

10. Use sua instalação do Helm local para criar todos os arquivos YAML do Kubernetes para seu gráfico do Helm. Os arquivos YAML são armazenados no diretório `output` que você criou anteriormente.
    ```
    gabarito do leme -- valores ./ibm-iks-cluster-autoscaler/values.yaml -- output-dir ./output ./ibm-iks-cluster-autoscaler
    ```
    {: pre}

    Saída de exemplo:
    ```
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-configmap.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service-account-roles.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-service.yaml
    wrote ./output/ibm-iks-cluster-autoscaler/templates/ca-deployment.yaml
    ```
    {: screen}

11. Implemente todos os arquivos YAML em seu cluster privado.
    ```
    kubectl apply -- recursive -- filename ./output
    ```
   {: pre}

12. Opcional: remova todos os arquivos YAML do diretório `output`.
    ```
    kubectl delete -- recursive -- filename ./output
    ```
    {: pre}

## Links relacionados do Helm
{: #helm_links}

Revise os links a seguir para localizar informações adicionais do Helm.
{: shortdesc}

* Visualize os gráficos do Helm disponíveis que podem ser usados no {{site.data.keyword.containerlong_notm}} no [Catálogo de gráficos do Helm ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/solutions/helm-charts).
* Saiba mais sobre os comandos do Helm que podem ser usados para configurar e gerenciar gráficos do Helm na <a href="https://docs.helm.sh/helm/" target="_blank">Documentação do Helm <img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a>.
* Saiba mais sobre como é possível [aumentar a velocidade de implementação com os gráficos Helm do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/recipes/tutorials/increase-deployment-velocity-with-kubernetes-helm-charts/).
