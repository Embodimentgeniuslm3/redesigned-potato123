---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, nginx, ingress controller

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


# Depurando o Ingresso
{: #cs_troubleshoot_debug_ingress}

Quando você usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas e depuração geral do Ingress.
{: shortdesc}

Você expôs publicamente seu app criando um recurso de Ingresso para seu app no cluster. No entanto, quando você tenta se conectar ao seu app por meio do endereço IP público ou subdomínio público do ALB, a conexão falha ou atinge o tempo limite. As etapas nas seções a seguir podem ajudar a depurar sua configuração do Ingress.

Assegure-se de definir um host em apenas um recurso do Ingress. Se um host for definido em múltiplos recursos do Ingress, o ALB poderá não encaminhar o tráfego corretamente e poderá haver erros.
{: tip}

Antes de iniciar, assegure-se de que você tenha as [políticas de acesso do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) a seguir:
  - A função da plataforma **Editor** ou **Administrador** para o cluster
  - Função de serviço ** Writer **  ou  ** Manager **

## Etapa 1: Executar testes do Ingress no {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool

Enquanto você soluciona problemas, é possível usar o {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool para executar testes do Ingress e reunir informações pertinentes de seu cluster. Para usar a ferramenta de depuração, instale o gráfico do Helm [`ibmcloud-iks-debug` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug):
{: shortdesc}


1. [Configure o Helm em seu cluster, crie uma conta do serviço para o Tiller e inclua o repositório `ibm` em sua instância do Helm](/docs/containers?topic=containers-helm).

2. Instale o gráfico Helm em seu cluster.
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. Inicie um servidor proxy para exibir a interface da ferramenta de depuração.
  ```
  kubectl proxy -- port 8080
  ```
  {: pre}

4. Em um navegador da web, abra a URL da interface de ferramenta de depuração: http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. Selecione o grupo  ** ingresso **  de testes. Alguns testes verificam potenciais avisos, erros ou problemas e alguns testes somente reúnem informações que podem ser referenciadas durante a resolução de problemas. Para obter mais informações sobre a função de cada teste, clique no ícone de informações próximo ao nome do teste.

6. Clique em  ** Executar **.

7. Verifique os resultados de cada teste.
  * Se algum teste falhar, clique no ícone de informações ao lado do nome do teste na coluna à esquerda para obter informações sobre como resolver o problema.
  * Também é possível usar os resultados de testes que apenas reúnem informações enquanto você depura o serviço Ingress nas seções a seguir.

## Etapa 2: Verifique as mensagens de erro em sua implementação de entrada e os logs pod ALB
{: #errors}

Inicie verificando se há mensagens de erro nos eventos de implementação do recurso Ingress e nos logs do pod do ALB. Essas mensagens de erro podem ajudá-lo a localizar as causas raiz para falhas e depurar ainda mais sua configuração do Ingress nas próximas seções.
{: shortdesc}

1. Verifique a sua implementação do recurso Ingress e procure mensagens de aviso ou erro.
    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    Na seção **Events** da saída, você pode ver mensagens de aviso sobre valores inválidos em seu recurso Ingress ou em certas anotações usadas. Verifique a [documentação de configuração do recurso Ingress](/docs/containers?topic=containers-ingress#public_inside_4) ou a [documentação de anotações](/docs/containers?topic=containers-ingress_annotation).

    ```
    Name:             myingress
    Namespace:        default
    Address:          169.xx.xxx.xxx,169.xx.xxx.xxx
    Default backend:  default-http-backend:80 (<none>)
    Rules:
      Host                                             Path  Backends
      ----                                             ----  --------
      mycluster.us-south.containers.appdomain.cloud
                                                       /tea      myservice1:80 (<none>)
                                                       /coffee   myservice2:80 (<none>)
    Annotations:
      custom-port:        protocol=http port=7490; protocol=https port=4431
      location-modifier:  modifier='~' serviceName=myservice1;modifier='^~' serviceName=myservice2
    Events:
      Type     Reason             Age   From                                                            Message
      ----     ------             ----  ----                                                            -------
      Normal   Success            1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  TLSSecretNotFound  1m    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress resource.
      Normal   Success            59s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Error annotation format error : One of the mandatory fields not valid/missing for annotation ingress.bluemix.net/custom-port
      Normal   Success            40s   public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
      Warning  AnnotationError    2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Failed to apply ingress.bluemix.net/custom-port annotation. Invalid port 7490. Annotation cannot use ports 7481 - 7490
      Normal   Success            2s    public-cr87c198fcf4bd458ca61402bb4c7e945a-alb1-258623678-gvf9n  Successfully applied ingress resource.
    ```
    {: screen}
{: #check_pods}
2. Verifique o status de seus pods do ALB.
    1. Obtenha os pods do ALB que estão em execução em seu cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Certifique-se de que todos os pods estejam em execução verificando a coluna **STATUS**.

    3. Se um pod não for `Running`, será possível desativar e ativar novamente o ALB. Nos comandos a seguir, substitua `<ALB_ID>` pelo ID do ALB do pod. Por exemplo, se o pod que não está em execução tiver o nome `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`, o ID do ALB será `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`.
        ```
        ibmcloud ks alb-configure -- albID < ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure -- albID < ALB_ID> -- enable
        ```
        {: pre}

3. Verifique os logs para o seu ALB.
    1.  Obtenha os IDs dos pods do ALB que estão em execução em seu cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. Obtenha os logs para o contêiner `nginx-ingress` em cada pod do ALB.
        ```
        Kubectl logs < ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. Procure mensagens de erro nos logs do ALB.

## Etapa 3: Executar ping do subdomínio ALB e endereços IP públicos
{: #ping}

Verifique a disponibilidade de seu subdomínio do Ingress e endereços IP públicos do ALB.
{: shortdesc}

1. Obtenha os endereços IP em que os ALBs públicos estão atendendo.
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Saída de exemplo para um cluster de múltiplas zonas com nós do trabalhador em `dal10` e `dal13`:

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * Se um ALB público não tiver nenhum endereço IP, consulte [O ALB do Ingress não é implementado em uma zona](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).

2. Verifique o funcionamento de seus IPs do ALB.

    * Para cluster de zona única e clusters de múltiplas zonas: execute ping do endereço IP de cada ALB público para assegurar que cada ALB esteja apto a receber pacotes com êxito. Se você estiver usando ALBs privados, será possível executar ping de seus endereços IP somente por meio da rede privada.
        ```
        ping <ALB_IP>
        ```
        {: pre}

        * Se a CLI retornar um tempo limite e você tiver um firewall customizado que esteja protegendo os nós do trabalhador, certifique-se de permitir o ICMP em seu [firewall](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall).
        * Se não houver nenhum firewall que esteja bloqueando os pings e os pings ainda forem executados até o tempo limite, [verifique o status de seus pods do ALB](#check_pods).

    * Somente clusters de múltiplas zonas: é possível usar a verificação de funcionamento do MZLB para determinar o status de seus IPs do ALB. Para obter mais informações sobre o MZLB, consulte [Multizone load balancer (MZLB)](/docs/containers?topic=containers-ingress#planning). A verificação de funcionamento MZLB está disponível apenas para clusters que possuem o novo subdomínio do Ingress no formato `<cluster_name>.<region_or_zone>.containers.appdomain.cloud`. Se seu cluster ainda usar o formato mais antigo, `<cluster_name>.<region>.containers.mybluemix.net`, [converta seu cluster de zona única para multizona](/docs/containers?topic=containers-add_workers#add_zone). Seu cluster é designado a um subdomínio com o novo formato, mas também pode continuar a usar o formato de subdomínio mais antigo. Como alternativa, é possível pedir um novo cluster que é designado automaticamente ao novo formato de subdomínio.

    O comando HTTP cURL a seguir usa o host `albhealth`, que é configurado pelo {{site.data.keyword.containerlong_notm}} para retornar o status `healthy` ou `unhealthy` para um IP do ALB.
        ```
        curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        Saída de exemplo:
        ```
        funcional
        ```
        {: screen}
        Se um ou mais IPs retornarem `unhealthy`, [verifique o status de seus pods do ALB](#check_pods).

3. Obtenha o subdomínio do Ingresso fornecido pela IBM.
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    Saída de exemplo:
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. Assegure-se de que os IPs para cada ALB público que você obteve na etapa 2 desta seção estejam registrados com o subdomínio do Ingress fornecido pela IBM de seu cluster. Por exemplo, em um cluster de múltiplas zonas, o IP do ALB público em cada zona em que você tem nós do trabalhador deve ser registrado sob o mesmo nome de host.

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    Saída de exemplo:
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## Etapa 4: Verificar os mapeamentos de domínio e a configuração do recurso Ingress
{: #ts_ingress_config}

1. Se você usar um domínio customizado, verifique se usou seu provedor DNS para mapear o domínio customizado para o subdomínio fornecido pela IBM ou o endereço IP público do ALB. Observe que usar um CNAME é preferencial porque a IBM fornece verificações automáticas de funcionamento no subdomínio IBM e remove os IPs com falha da resposta de DNS.
    * Subdomínio fornecido pela IBM: verifique se seu domínio customizado está mapeado para o subdomínio fornecido pela IBM do cluster no registro Canonical Name (CNAME).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Saída de exemplo:
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * Endereço IP público: verifique se seu domínio customizado está mapeado para o endereço IP público móvel do ALB no registro A. Os IPs devem corresponder aos IPs públicos do ALB que você obteve na etapa 1 da [seção anterior](#ping).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Saída de exemplo:
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. Verifique os arquivos de configuração do recurso Ingress para seu cluster.
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. Assegure-se de definir um host em apenas um recurso do Ingress. Se um host for definido em múltiplos recursos do Ingress, o ALB poderá não encaminhar o tráfego corretamente e poderá haver erros.

    2. Verifique se o subdomínio e o certificado TLS estão corretos. Para localizar o subdomínio do Ingress fornecido pela IBM e o certificado TLS, execute `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.

    3.  Certifique-se de que seu app atenda no mesmo caminho configurado na seção de **caminho** de seu Ingresso. Se seu app estiver configurado para atender no caminho raiz, use `/` como o caminho. Se o tráfego recebido para esse caminho deve ser roteado para um caminho diferente no qual seu app atende, use a anotação [caminhos de nova gravação](/docs/containers?topic=containers-ingress_annotation#rewrite-path).

    4. Edite seu YAML de configuração de recurso, conforme necessário. Quando você fecha o editor, suas mudanças são salvas e aplicadas automaticamente.
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## Removendo um ALB do DNS para depuração
{: #one_alb}

Caso não seja possível acessar seu app por meio de um IP específico do ALB, será possível remover temporariamente o ALB da produção desativando seu registro de DNS. Em seguida, será possível usar o endereço IP do ALB para executar testes de depuração nesse ALB.

Por exemplo, vamos supor que você tenha um cluster de múltiplas zonas em 2 zonas e os 2 ALBs públicos tenham endereços IP `169.46.52.222` e `169.62.196.238`. Embora a verificação de funcionamento esteja retornando como funcional para o ALB da segunda zona, seu app não pode ser atingido diretamente por meio dele. Você decide remover o endereço IP do ALB, `169.62.196.238`, da produção para depuração. O IP do ALB da primeira zona, `169.46.52.222`, é registrado com seu domínio e continua a rotear o tráfego enquanto você depura o ALB da segunda zona.

1. Obtenha o nome do ALB com o endereço IP inatingível.
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    Por exemplo, o IP inatingível `169.62.196.238` pertence ao ALB `public-cr24a9f2caf6554648836337d240064935-alb1`:
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. Usando o nome do ALB da etapa anterior, obtenha os nomes dos pods do ALB. O comando a seguir usa o nome do ALB de exemplo da etapa anterior:
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    Saída de exemplo:
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. Desative a verificação de funcionamento que é executada para todos os pods do ALB. Repita essas etapas para cada pod do ALB que você obteve na etapa anterior. Os comandos e a saída de exemplo nestas etapas usam o primeiro pod, `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`.
    1. Efetue login no pod do ALB e verifique a linha `server_name` no arquivo de configuração do NGINX.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Saída de exemplo que confirma que o pod do ALB está configurado com o nome do host de verificação de funcionamento correto, `albhealth.<domain>`:
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. Para remover o IP desativando a verificação de funcionamento, insira `#` na frente do `server_name`. Quando o host virtual `albhealth.mycluster-12345.us-south.containers.appdomain.cloud ` está desativado para o ALB, a verificação de funcionamento automatizada remove automaticamente o IP da resposta de DNS.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. Verifique se a mudança foi aplicada.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Saída de exemplo:
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. Para remover o IP do registro de DNS, recarregue a configuração do NGINX.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. Repita essas etapas para cada pod ALB.

4. Agora, quando você tenta executar cURL no host `albhealth` para verificação de funcionamento do IP do ALB, a verificação falha.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    Saída:
    ```
    <html>
        <head>
            <title>404 Não Localizado</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Não Localizado</h1>
        </body>
    </html>
    ```
    {: screen}

5. Verifique se o endereço IP do ALB é removido do registro de DNS para seu domínio, verificando o servidor Cloudflare. Observe que o registro de DNS pode levar alguns minutos para ser atualizado.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Saída de exemplo que confirma que somente o IP do ALB funcional, `169.46.52.222`, permanece no registro de DNS e que o IP ALB não funcional, `169.62.196.238`, foi removido:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud tem o endereço 169.46.52.222
    ```
    {: screen}

6. Agora que o IP do ALB foi removido da produção, é possível executar testes de depuração com relação ao seu app por meio dele. Para testar a comunicação com seu app por meio desse IP, é possível executar o comando cURL a seguir, substituindo os valores de exemplo por seus próprios valores:
    ```
    curl -X GET -- resolve mycluster-12345.us-south.containers.appdomain.cloud: 443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * Se tudo estiver configurado corretamente, você obterá de volta a resposta esperada de seu app.
    * Se você obtiver um erro em resposta, poderá haver um erro em seu app ou em uma configuração que se aplique somente a esse ALB específico. Verifique seu código de app, seus [arquivos de configuração do recurso Ingress](/docs/containers?topic=containers-ingress#public_inside_4) ou quaisquer outras configurações que você tenha aplicado somente a esse ALB.

7. Depois de concluir a depuração, restaure a verificação de funcionamento nos pods do ALB. Repita essas etapas para cada pod ALB.
  1. Efetue login no pod do ALB e remova o `#` do `server_name`.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. Recarregue a configuração do NGINX para que a restauração de verificação de funcionamento seja aplicada.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. Agora, quando você executar cURL do host `albhealth` para a verificação de funcionamento do IP do ALB, a verificação retornará `healthy`.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Verifique se o endereço IP do ALB foi restaurado no registro de DNS para seu domínio, verificando o servidor Cloudflare. Observe que o registro de DNS pode levar alguns minutos para ser atualizado.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Saída de exemplo:
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## Obtendo ajuda e suporte
{: #ingress_getting_help}

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

