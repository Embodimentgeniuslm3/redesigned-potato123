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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Nuvem híbrida
{: #hybrid_iks_icp}

Se você tiver uma conta {{site.data.keyword.Bluemix}} Private, será possível usá-la com os serviços do {{site.data.keyword.Bluemix_notm}} selecionados, incluindo o {{site.data.keyword.containerlong}}. Para obter mais informações, consulte o blog em [Experiência híbrida no {{site.data.keyword.Bluemix_notm}} Private e IBM Public Cloud![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](http://ibm.biz/hybridJune2018).
{: shortdesc}

Você entende as [ofertas do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_ov#differentiation) e desenvolveu sua estratégia do Kubernetes para quais [cargas de trabalho devem ser executadas na nuvem](/docs/containers?topic=containers-strategy#cloud_workloads). Agora, é possível conectar sua nuvem pública e privada usando o serviço VPN do strongSwan ou o {{site.data.keyword.BluDirectLink}}.

* O [serviço VPN do strongSwan](#hybrid_vpn) conecta de forma segura seu cluster Kubernetes a uma rede no local por meio de um canal de comunicação de ponta a ponta seguro pela Internet que é baseada no conjunto de protocolos Internet Protocol Security (IPSec) padrão de mercado.
* Com o [{{site.data.keyword.Bluemix_notm}} Direct Link](#hybrid_dl), é possível criar uma conexão direta e privada entre seus ambientes de rede remota e o {{site.data.keyword.containerlong_notm}} sem rotear pela Internet pública.

Depois de conectar sua nuvem pública e privada, é possível [reutilizar seus pacotes privados para contêineres públicos](#hybrid_ppa_importer).

## Conectando sua nuvem pública e privada com a VPN strongSwan
{: #hybrid_vpn}

Estabeleça a conectividade de VPN entre o cluster público do Kubernetes e sua instância do {{site.data.keyword.Bluemix}} Private para permitir a comunicação bidirecional.
{: shortdesc}

1.  Crie um cluster padrão com o {{site.data.keyword.containerlong}} no {{site.data.keyword.Bluemix_notm}} Public ou use um existente. Para criar um cluster, escolha entre as opções a seguir:
    - [Crie um cluster padrão por meio do console ou da CLI](/docs/containers?topic=containers-clusters#clusters_ui).
    - [ Use o Cloud Automation Manager (CAM) para criar um cluster usando um modelo predefinido![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_deploy_IKS.html). Ao implementar um cluster com o CAM, o Helm tiller é instalado automaticamente para você.

2.  Em seu cluster do {{site.data.keyword.containerlong_notm}}, [siga as instruções para configurar o serviço de VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn_configure).

    *  Para a  [ Etapa 2 ](/docs/containers?topic=containers-vpn#strongswan_2), observe que:

       * O `local.id ` que você configurar em seu cluster do {{site.data.keyword.containerlong_notm}} deverá corresponder ao que configurar posteriormente como o `remote.id` em seu cluster do {{site.data.keyword.Bluemix}} Private.
       * O `remote.id` que você configurar em seu cluster do {{site.data.keyword.containerlong_notm}} deverá corresponder ao que configurar posteriormente como o `local.id` em seu cluster do {{site.data.keyword.Bluemix}} Private.
       * O `preshared.secret` que você configurar em seu cluster do {{site.data.keyword.containerlong_notm}} deverá corresponder ao que configurar posteriormente como o `preshared.secret` em seu cluster do {{site.data.keyword.Bluemix}} Private.

    *  Para a [Etapa 3](/docs/containers?topic=containers-vpn#strongswan_3), configure o strongSwan para uma conexão VPN de **entrada**.

       ```
       ipsec.auto: add
       loadBalancerIP: <portable_public_IP>
       ```
       {: codeblock}

3.  Anote o endereço IP público móvel que você configurar como o `loadbalancerIP`.

    ```
    kubectl get svc vpn-strongswan
    ```
    {: pre}

4.  [Crie um cluster no {{site.data.keyword.Bluemix_notm}} Private![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/installing/installing.html).

5.  Em seu cluster do {{site.data.keyword.Bluemix_notm}} Private, implemente o serviço de VPN IPSec strongSwan.

    1.  [Conclua as soluções alternativas de VPN IPSec strongSwan ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SS2L37_2.1.0.3/cam_strongswan.html).

    2.  [Configure o gráfico Helm de VPN strongSwan ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/SSBS6K_2.1.0.3/app_center/create_release.html) em seu cluster privado.

        *  Nos parâmetros de configuração, configure o campo **Gateway remoto** para o valor do endereço IP público móvel que você configurar como o `loadbalancerIP` de seu cluster do {{site.data.keyword.containerlong_notm}}.

           ```
           Operation at startup: start
           ...
           Remote gateway: <portable_public_IP>
           ...
           ```
           {: codeblock}

        *  Lembre-se de que o `local.id` privado deve corresponder ao `remote.id` público, o `remote.id` privado deve corresponder ao `local.id` público e os valores `preshared.secret` para privado e público devem corresponder.

        Agora, é possível iniciar uma conexão do cluster do {{site.data.keyword.Bluemix_notm}} Private para o cluster do {{site.data.keyword.containerlong_notm}}.

7.  [Teste a conexão VPN](/docs/containers?topic=containers-vpn#vpn_test) entre seus clusters.

8.  Repita essas etapas para cada cluster que você deseja conectar.

**O que vem a seguir?**

*   [Execute as imagens de seu software licenciado em clusters públicos](#hybrid_ppa_importer).
*   Para gerenciar múltiplos clusters do Kubernetes em nuvem, como ao longo do {{site.data.keyword.Bluemix_notm}} Public e do {{site.data.keyword.Bluemix_notm}} Private, efetue check-out do [IBM Multicloud Manager ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).


## Conectando sua nuvem pública e privada com o {{site.data.keyword.Bluemix_notm}} Direct Link
{: #hybrid_dl}

Com o [{{site.data.keyword.BluDirectLink}}](/docs/infrastructure/direct-link?topic=direct-link-about-ibm-cloud-direct-link), é possível criar uma conexão direta e privada entre seus ambientes de rede remota e o {{site.data.keyword.containerlong_notm}} sem rotear pela Internet pública.
{: shortdesc}

Para conectar sua nuvem pública e sua instância do {{site.data.keyword.Bluemix}} Private no local, é possível usar uma das quatro ofertas:
* {{site.data.keyword.Bluemix_notm}} Direct Link Connect
* {{site.data.keyword.Bluemix_notm}} Direct Link Exchange
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated
* {{site.data.keyword.Bluemix_notm}} Direct Link Dedicated Hosting

Para escolher uma oferta {{site.data.keyword.Bluemix_notm}} Direct Link e configurar uma conexão do {{site.data.keyword.Bluemix_notm}} Direct Link, consulte [Introdução ao {{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link#how-do-i-know-which-type-of-ibm-cloud-direct-link-i-need-) na documentação do {{site.data.keyword.Bluemix_notm}} Direct Link.

**O que Vem a Seguir?**</br>
* [Execute as imagens de seu software licenciado em clusters públicos](#hybrid_ppa_importer).
* Para gerenciar múltiplos clusters do Kubernetes em nuvem, como ao longo do {{site.data.keyword.Bluemix_notm}} Public e do {{site.data.keyword.Bluemix_notm}} Private, efetue check-out do [IBM Multicloud Manager ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_3.1.0/mcm/getting_started/introduction.html).

<br />


## Executando imagens do {{site.data.keyword.Bluemix_notm}} Private em contêineres públicos do Kubernetes
{: #hybrid_ppa_importer}

É possível executar produtos IBM licenciados selecionados que foram empacotados para o {{site.data.keyword.Bluemix_notm}} Private em um cluster no {{site.data.keyword.Bluemix_notm}} Public.  
{: shortdesc}

O software licenciado está disponível no [IBM Passport Advantage ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/software/passportadvantage/index.html). Para usar esse software em um cluster no {{site.data.keyword.Bluemix_notm}} Public, deve-se fazer download do software, extrair a imagem e fazer upload da imagem para seu namespace no {{site.data.keyword.registryshort}}. Independentemente do ambiente no qual você planeja usar o software, deve-se obter a licença necessária para o produto primeiro.

A tabela a seguir é uma visão geral de produtos disponíveis do {{site.data.keyword.Bluemix_notm}} Private que podem ser usados em seu cluster no {{site.data.keyword.Bluemix_notm}} Public.

| Nome do produto | Versão | Número de Peça |
| --- | --- | --- |
| IBM Db2 Direct Advanced Edition Server | 11,1 | CNU3TML |
| IBM Db2 Advanced Enterprise Server Edition Server | 11,1 | CNU3SML |
| IBM MQ Advanced | 9.1.0.0, 9.1.1,0, 9.1.2.0 | - |
| IBM WebSphere Application Server Liberty | 16.0.0.3 | Imagem do Docker Hub |
{: caption="Tabela. Produtos suportados do {{site.data.keyword.Bluemix_notm}} Private para serem usados no {{site.data.keyword.Bluemix_notm}} Public." caption-side="top"}

Antes de iniciar:
- [Instale o plug-in da CLI do {{site.data.keyword.registryshort}} (`ibmcloud cr`)](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#cli_namespace_registry_cli_install).
- [Configure um namespace no {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-registry_setup_cli_namespace#registry_namespace_setup) ou recupere seu namespace existente executando `ibmcloud cr namespaces`.
- [Destine a sua CLI `kubectl` para o seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
- [Instale a CLI do Helm e configure o tiller em seu cluster](/docs/containers?topic=containers-helm#public_helm_install).

Para implementar uma imagem do {{site.data.keyword.Bluemix_notm}} Private em um cluster no {{site.data.keyword.Bluemix_notm}} Public:

1.  Siga as etapas na [documentação do {{site.data.keyword.registryshort}}](/docs/services/Registry?topic=registry-ts_index#ts_ppa) para fazer download do software licenciado por meio do IBM Passport Advantage, enviar por push a imagem para seu namespace e instalar o gráfico do Helm em seu cluster.

    ** Para o IBM WebSphere Application Server Liberty **:

    1.  Em vez de obter a imagem por meio do IBM Passport Advantage, use a [imagem do Docker Hub ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://hub.docker.com/_/websphere-liberty/). Para obter instruções sobre como obter uma licença de produção, consulte [Fazendo upgrade da imagem do Docker Hub para uma imagem de produção![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/WASdev/ci.docker/tree/master/ga/production-upgrade).

    2.  Siga as [instruções do gráfico Helm do Liberty![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/com.ibm.websphere.wlp.doc/ae/rwlp_icp_helm.html).

2.  Verifique se o **STATUS** do gráfico Helm mostra `DEPLOYED`. Se não, aguarde alguns minutos e, em seguida, tente novamente.
    ```
    helm status <helm_chart_name>
    ```
    {: pre}

3.  Consulte a documentação específica do produto para obter mais informações sobre como configurar e usar o produto com seu cluster.

    - [IBM Db2 Direct Advanced Edition Server ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.licensing.doc/doc/c0070181.html)
    - [IBM MQ Advanced ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSFKSJ_9.0.0/com.ibm.mq.helphome.v90.doc/WelcomePagev9r0.html)
    - [IBM WebSphere Application Server Liberty ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/support/knowledgecenter/en/SSEQTP_liberty/as_ditamaps/was900_welcome_liberty.html)
