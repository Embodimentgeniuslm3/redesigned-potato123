---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, clusters, worker nodes, worker pools, delete

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
{:gif: data-image-type='gif'}


# Removendo clusters
{: #remove}

Os clusters grátis e padrão que são criados com uma conta faturável devem ser removidos manualmente quando eles não forem mais necessários para que esses clusters não estejam mais consumindo recursos.
{:shortdesc}

<p class="important">
Nenhum backup é criado de seu cluster ou dos dados no armazenamento persistente. Ao excluir um cluster, é possível optar por excluir seu armazenamento persistente. O armazenamento persistente que você forneceu usando uma classe de armazenamento `delete` será excluído permanentemente na infraestrutura do IBM Cloud (SoftLayer) se você optar por excluir seu armazenamento persistente. Se você provisionou seu armazenamento persistente usando uma classe de armazenamento `retain` e optar por excluí-lo, o cluster, o PV e a PVC serão excluídos, mas a instância do armazenamento persistente em sua conta da infraestrutura do IBM Cloud (SoftLayer) seja mantida.</br>
</br>Quando você remove um cluster, também remove quaisquer sub-redes provisionadas automaticamente durante sua criação e criadas por meio do comando `ibmcloud ks cluster-subnet-create`. No entanto, se você incluiu manualmente as sub-redes existentes em seu cluster usando o comando `ibmcloud ks cluster-subnet-subnet-add`, essas sub-redes não são removidas de sua conta de infraestrutura do IBM Cloud (SoftLayer) e é possível reutilizá-las em outros clusters.</p>

Antes de iniciar:
* Anote seu ID do cluster. Você pode precisar do ID do cluster para investigar e remover os recursos da infraestrutura do IBM Cloud (SoftLayer) relacionados que não são excluídos automaticamente com seu cluster.
* Se você desejar excluir os dados em seu armazenamento persistente, [entenda as opções de exclusão](/docs/containers?topic=containers-cleanup#cleanup).
* Certifique-se de que você tenha a [função da plataforma **Administrator** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform).

Para remover um cluster:

1. Opcional: na CLI, salve uma cópia de todos os dados em seu cluster em um arquivo YAML local.
  ```
  kubectl get all --all-namespaces -o yaml
  ```
  {: pre}

2. Remova o cluster.
  - No console do {{site.data.keyword.Bluemix_notm}}
    1.  Selecione seu cluster e clique em **Excluir** no menu **Mais ações...**.

  - No {{site.data.keyword.Bluemix_notm}} CLI
    1.  Liste os clusters disponíveis.

        ```
        ibmcloud ks clusters
        ```
        {: pre}

    2.  Exclua o cluster.

        ```
        ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
        ```
        {: pre}

    3.  Siga os prompts e escolha se deseja excluir recursos de cluster, que inclui contêineres, pods, serviços de limite, armazenamento persistente e segredos.
      - **Armazenamento persistente**: o armazenamento persistente fornece alta disponibilidade para seus dados. Se você criou uma solicitação de volume persistente usando um [compartilhamento de arquivo existente](/docs/containers?topic=containers-file_storage#existing_file), não será possível excluir o compartilhamento de arquivo quando excluir o cluster. Deve-se excluir manualmente o compartilhamento de arquivo posteriormente de seu portfólio de infraestrutura do IBM Cloud (SoftLayer).

          Devido ao ciclo de faturamento mensal, uma solicitação de volume persistente não pode ser excluída no último dia de um mês. Se você excluir a solicitação de volume persistente no último dia do mês, a exclusão
permanecerá pendente até o início do mês seguinte.
          {: note}

Próximas etapas:
- Depois que ele não estiver mais listado na lista de clusters disponíveis quando você executar o comando `ibmcloud ks clusters`, será possível reutilizar o nome de um cluster removido.
- Se você tiver mantido as sub-redes, será possível [reutilizá-las em um novo cluster](/docs/containers?topic=containers-subnets#subnets_custom) ou excluí-las manualmente mais tarde de seu portfólio de infraestrutura do IBM Cloud (SoftLayer).
- Se você manteve o armazenamento persistente, será possível [excluir seu armazenamento](/docs/containers?topic=containers-cleanup#cleanup) posteriormente por meio do painel de infraestrutura do IBM Cloud (SoftLayer) no console do {{site.data.keyword.Bluemix_notm}}.
