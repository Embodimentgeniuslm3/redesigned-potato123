---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

keywords: kubernetes, iks, multi az, multi-az, szr, mzr

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



# Planejando a configuração do nó do trabalhador
{: #planning_worker_nodes}

Um cluster do Kubernetes consiste em nós do trabalhador que são agrupados em conjuntos de nós do trabalhador e são monitorados e gerenciados centralmente pelo mestre do Kubernetes. Os administradores de cluster decidem como configurar o cluster de nós do trabalhador para assegurar que os usuários do cluster tenham todos os recursos para implementar e executar apps no cluster.
{:shortdesc}

Ao criar um cluster padrão, os nós do trabalhador com as mesmas especificações (tipo) de memória, CPU e espaço em disco são solicitados na infraestrutura do IBM Cloud (SoftLayer) em seu nome e incluídos no conjunto de nós do trabalhador padrão em seu cluster. A cada nó do trabalhador é designado
um ID de nó do trabalhador e um nome de domínio exclusivos que não devem ser mudados após a criação do cluster. É possível escolher entre servidores virtuais ou físicos (bare metal). Dependendo do nível de isolamento de hardware escolhido, os nós do trabalhador virtual podem ser configurados como nós compartilhados ou dedicados. Para incluir diferentes tipos em seu cluster, [crie outro conjunto de trabalhadores](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_pool_create).

O Kubernetes limita o número máximo de nós do trabalhador que você pode ter em um cluster. Revise [cotas de nó do trabalhador e de pod ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/setup/cluster-large/) para obter mais informações.


Deseja certificar-se de sempre ter nós do trabalhador suficientes para cobrir sua carga de trabalho? Experimente  [ o cluster autoscaler ](/docs/containers?topic=containers-ca#ca).
{: tip}

<br />


## Hardware disponível para nós do trabalhador
{: #shared_dedicated_node}

Ao criar um cluster padrão no {{site.data.keyword.Bluemix_notm}}, você escolhe se os conjuntos de trabalhadores consistem em nós do trabalhador que são máquinas físicas (bare metal) ou máquinas virtuais que são executadas em hardware físico. Você também seleciona o tipo de nó do trabalhador ou a combinação de memória, CPU e outras especificações de máquina, como armazenamento em disco.
{:shortdesc}

<img src="images/cs_clusters_hardware.png" width="700" alt="Opções de hardware para os nós do trabalhador em um cluster padrão" style="width:700px; border-style: none"/>

Se você deseja mais de um tipo de nó do trabalhador, deve-se criar um conjunto de trabalhadores para cada tipo. Não é possível redimensionar os nós do trabalhador existentes para que tenham recursos diferentes, como CPU ou memória. Ao criar um cluster grátis, seu nó do trabalhador é provisionado automaticamente como um nó virtual compartilhado na conta de infraestrutura do IBM Cloud (SoftLayer). Em clusters padrão, é possível escolher o tipo de máquina que funciona melhor para sua carga de trabalho. Conforme você planeja, considere as [reservas de recurso do nó do trabalhador](#resource_limit_node) na capacidade total de CPU e memória.

Selecione uma das opções a seguir para decidir qual tipo de conjunto de trabalhadores você deseja.
* [Máquinas virtuais](#vm)
* [Máquinas físicas (bare metal)](#bm)
* [Máquinas de armazenamento definido pelo software (SDS)](#sds)

## Máquinas virtuais
{: #vm}

Com as VMs, você tem maior flexibilidade, tempos de fornecimento mais rápidos e recursos de escalabilidade mais automáticos do que o bare metal, com custo reduzido. É possível usar VMs para os casos de uso geral, como ambientes de teste e desenvolvimento, ambientes de preparação e produção, microsserviços e apps de negócios. No entanto, há alternativas no desempenho. Se você precisar de computação de alto desempenho para cargas de trabalho com uso intensivo de RAM, dados ou GPU, use [bare metal](#bm).
{: shortdesc}

**Desejo usar hardware compartilhado ou dedicado?**</br>
Ao criar um cluster virtual padrão, deve-se escolher se deseja que o hardware subjacente seja compartilhado por múltiplos clientes {{site.data.keyword.IBM_notm}} (ocupação variada) ou seja dedicado somente a você (ocupação única).

* **Em uma configuração de hardware compartilhado de diversos locatários**: os recursos físicos, como CPU e memória, são compartilhados entre todas as máquinas virtuais que são implementadas no mesmo hardware físico. Para assegurar que cada máquina
virtual possa ser executada independentemente, um monitor de máquina virtual, também referido como hypervisor,
segmenta os recursos físicos em entidades isoladas e aloca como recursos dedicados para
uma máquina virtual (isolamento de hypervisor).
* **Em uma configuração de hardware dedicado de locatário único**: todos os recursos físicos são dedicados somente a você. É possível implementar
múltiplos nós do trabalhador como máquinas virtuais no mesmo host físico. Semelhante à configuração de diversos locatários, o hypervisor assegura que cada nó do trabalhador obtenha seu compartilhamento dos recursos físicos disponíveis.

Os nós compartilhados são geralmente menos dispendiosos que os nós dedicados porque os custos para o hardware subjacente são compartilhados entre múltiplos clientes. No entanto, ao decidir entre nós compartilhados
e dedicados, você pode desejar verificar com seu departamento jurídico para discutir o nível de isolamento
e conformidade de infraestrutura que seu ambiente de app requer.

Alguns tipos estão disponíveis para apenas um tipo de configuração de ocupação. Por exemplo, as VMs `m3c` estão disponíveis apenas como uma configuração de ocupação `shared`.
{: note}

**Quais são os recursos gerais das VMs?**</br>
As máquinas virtuais usam o disco local em vez da rede de área de armazenamento (SAN) para confiabilidade. Os benefícios de confiabilidade incluem maior rendimento ao serializar bytes para o disco local e a degradação do sistema de arquivos reduzido devido a falhas de rede. Cada VM é fornecida com 1000 Mbps de velocidade de rede, 25 GB de armazenamento em disco local primário para o sistema de arquivos do S.O. e 100 GB de armazenamento em disco local secundário para dados, como o tempo de execução do contêiner e o `kubelet`. O armazenamento local no nó do trabalhador é somente para processamento de curto prazo e os discos primário e secundário são limpos quando você atualiza ou recarrega o nó do trabalhador. Para obter soluções de armazenamento persistente, consulte [Planejando o armazenamento persistente altamente disponível](/docs/containers?topic=containers-storage_planning#storage_planning).

**E se eu tiver tipos de máquina mais antigos?**</br>
Se o seu cluster tiver tipos de nó do trabalhador descontinuados `x1c` ou outros tipos mais antigos de nós do trabalhador `x2c` do Ubuntu 16, será possível [atualizar seu cluster para ter nós do trabalhador `x3c`](/docs/containers?topic=containers-update#machine_type) do Ubuntu 18.

**Quais tipos de máquina virtual estão disponíveis?**</br>
Os tipos de nó do trabalhador variam por zona. A tabela a seguir inclui a versão mais recente de um tipo, como tipos de nós do trabalhador `x3c` do Ubuntu 18, em oposição aos tipos de nó do trabalhador `x2c` do Ubuntu 16 mais antigos. Para ver os tipos de máquina disponíveis em sua zona, execute `ibmcloud ks machine-types <zone>`. Também é possível revisar os tipos de máquina [bare metal](#bm) ou [SDS](#sds) disponíveis.

{: #vm-table}
<table>
<caption>Tipos de máquina virtual disponíveis no  {{site.data.keyword.containerlong_notm}}.</caption>
<thead>
<th>Caso Nome e uso</th>
<th>Núcleos / Memória</th>
<th>Disco Primário / Secundário</th>
<th>Velocidade</th>
</thead>
<tbody>
<tr>
<td><strong>Virtual, u3c.2x4</strong>: use essa VM menor para testes rápido, provas de conceito e outras cargas de trabalho leves.</td>
<td>2/4 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b3c.4x16</strong>: selecione essa VM balanceada para testes, desenvolvimento e outras cargas de trabalho leves.</td>
<td>4/16 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b3c.16x64</strong>: selecione essa VM balanceada para cargas de trabalho de médio porte.</td></td>
<td>16/64 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>Virtual, b3c.32x128</strong>: selecione essa VM balanceada para cargas de trabalho de médio a grande porte, como um banco de dados e um website dinâmico com muitos usuários simultâneos.</td>
<td>32/128 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>Virtual, c3c.16x16</strong>: use esse tipo quando desejar um balanceamento uniforme de recursos de cálculo do nó do trabalhador para cargas de trabalho leves.</td>
<td>16/16 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr><tr>
<td><strong>Virtual, c3c.16x32</strong>: use esse tipo quando desejar uma proporção 1:2 de recursos de CPU e memória do nó do trabalhador para cargas de trabalho de pequeno a médio porte.</td>
<td>16/32 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr><tr>
<td><strong>Virtual, c3c.32x32</strong>: use esse tipo quando desejar um balanceamento uniforme de recursos de cálculo do nó do trabalhador para cargas de trabalho de médio porte.</td>
<td>32/32 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr><tr>
<td><strong>Virtual, c3c.32x64</strong>: use esse tipo quando desejar uma proporção 1:2 de recursos de CPU e memória do nó trabalhador para cargas de trabalho de médio porte.</td>
<td>32/64 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>Virtual, m3c.8x64</strong>: use esse tipo quando desejar uma proporção 1:8 de recursos de CPU e memória para cargas de trabalho de pequeno a médio porte que requerem mais memória, semelhante a bancos de dados como o {{site.data.keyword.Db2_on_Cloud_short}}. Disponível somente em Dallas e como uma ocupação `--hardware shared`.</td>
<td>8/64 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr><tr>
<td><strong>Virtual, m3c.16x128</strong>: use esse tipo quando desejar uma proporção 1:8 de recursos de CPU e memória para cargas de trabalho de médio porte que requerem mais memória, semelhante a bancos de dados como {{site.data.keyword.Db2_on_Cloud_short}}. Disponível somente em Dallas e como uma ocupação `--hardware shared`.</td>
<td>16/128 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr><tr>
<td><strong>Virtual, m3c.30x240</strong>: use esse tipo quando desejar uma proporção 1:8 de recursos de CPU e memória para cargas de trabalho de médio a grande porte que requerem mais memória, semelhante a bancos de dados como {{site.data.keyword.Db2_on_Cloud_short}}. Disponível somente em Dallas e como uma ocupação `--hardware shared`.</td>
<td>30/240 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr>
<tr>
<td><strong>Virtual, z1.2x4</strong>: use esse tipo quando desejar que um nó trabalhador seja criado no Hyper Protect Containers no IBM Z Systems.</td>
<td>2/4 GB</td>
<td>25 GB/100 GB</td>
<td>1000 Mbps</td>
</tr>
</tbody>
</table>

## Máquinas físicas (bare metal)
{: #bm}

É possível provisionar o nó do trabalhador como um servidor físico de único locatário, também referido como bare metal.
{: shortdesc}

**Qual a diferença entre o bare metal e as VMs?**</br>
O bare metal dá acesso direto aos recursos físicos na máquina, como a memória ou CPU. Essa configuração elimina o hypervisor da máquina virtual que aloca recursos físicos para máquinas virtuais executadas no host. Em vez disso, todos os recursos de uma máquina bare metal são dedicados exclusivamente ao trabalhador, portanto, você não precisará se preocupar com "vizinhos barulhentos" compartilhando recursos ou diminuindo o desempenho. Os tipos de máquina física têm mais armazenamento local do que virtual e alguns têm RAID para aumentar a disponibilidade de dados. O armazenamento local no nó do trabalhador é somente para processamento de curto prazo e os discos primário e secundário são limpos quando você atualiza ou recarrega o nó do trabalhador. Para obter soluções de armazenamento persistente, consulte [Planejando o armazenamento persistente altamente disponível](/docs/containers?topic=containers-storage_planning#storage_planning).

**Além de melhores especificações para o desempenho, posso fazer algo com bare metal que não posso com VMs?**</br>
Sim. Com bare metal, você tem a opção de ativar o Cálculo Confiável para verificar seus nós do trabalhador com relação à violação. Se você não ativar a confiança durante a criação do cluster, mas desejar fazer isso posteriormente, será possível usar o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_feature_enable) `ibmcloud ks feature-enable`. Depois de ativar a confiança, não é possível desativá-la posteriormente. É possível fazer um novo cluster sem confiança. Para obter mais informações sobre como a confiança funciona durante o processo de inicialização do nó, veja [{{site.data.keyword.containerlong_notm}} com Cálculo confiável](/docs/containers?topic=containers-security#trusted_compute). O Trusted Compute está disponível para determinados tipos de máquina bare metal. Quando você executa o [comando](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_machine_types) `ibmcloud ks machine-types <zone>`, é possível ver quais máquinas suportam confiança revisando o campo **Confiável**. Por exemplo, os tipos GPU `mgXc` não suportam o Cálculo confiável.

Além do Trusted Compute, também é possível aproveitar o {{site.data.keyword.datashield_full}} (Beta). O {{site.data.keyword.datashield_short}} está integrado às tecnologias Intel® Software Guard Extensions (SGX) e Fortanix® para que seu código de carga de trabalho de contêiner do {{site.data.keyword.Bluemix_notm}} e os dados sejam protegidos em uso. O código do app e os dados são executados em enclaves reforçados pela CPU, que são áreas confiáveis de memória no nó do trabalhador que protegem os aspectos críticos do app, o que ajuda a manter o código e os dados confidenciais e não modificados. Se você ou sua empresa requer sensibilidade de dados devido a políticas internas, regulamentações governamentais ou requisitos de conformidade da indústria, essa solução pode ajudá-lo a mover-se para a nuvem. Os casos de uso de exemplo incluem instituições financeiras e de assistência médica ou países com políticas governamentais que requerem soluções de nuvem no local.

** Bare metal parece incrível! Por que não solicito um agora mesmo?**</br>
Os servidores bare metal são mais caros do que os servidores virtuais e são mais adequados para apps de alto desempenho que precisam de mais recursos e controle de host.

Os servidores bare metal são faturados mensalmente. Se você cancelar um servidor bare metal antes do final do mês, será cobrado até o final do mês. Depois de pedir ou cancelar um servidor bare metal, o processo é concluído manualmente em sua conta de infraestrutura do IBM Cloud (SoftLayer). Portanto, isso pode levar mais de um dia útil para ser concluído.
{: important}

**Quais tipos de bare metal posso solicitar?**</br>
Os tipos de nó do trabalhador variam por zona. A tabela a seguir inclui a versão mais recente de um tipo, como tipos de nós do trabalhador `x3c` do Ubuntu 18, em oposição aos tipos de nó do trabalhador `x2c` do Ubuntu 16 mais antigos. Para ver os tipos de máquina disponíveis em sua zona, execute `ibmcloud ks machine-types <zone>`. Também é possível revisar os tipos de máquina [VM](#vm) ou [SDS](#sds) disponíveis.

As máquinas bare metal são otimizadas para diferentes casos de uso, como cargas de trabalho intensivas de RAM, de dados ou de GPU.

Escolha um tipo de máquina com a configuração de armazenamento correta para suportar sua carga de trabalho. Alguns tipos têm uma combinação dos discos e configurações de armazenamento a seguir. Por exemplo, alguns tipos podem ter um disco primário SATA com um disco secundário SSD bruto.

* **SATA**: um dispositivo de armazenamento em disco de rotação magnética que é usado frequentemente para o disco primário do nó do trabalhador que armazena o sistema de arquivos de S.O.
* **SSD**: um dispositivo de armazenamento de unidade de estado sólido para dados de alto desempenho.
* **Bruto**: o dispositivo de armazenamento não está formatado, com a capacidade total disponível para uso.
* **RAID**: o dispositivo de armazenamento tem dados distribuídos para redundância e desempenho que variam dependendo do nível do RAID. Como tal, a capacidade do disco que está disponível para uso varia.


{: #bm-table}
<table>
<caption>Tipos de máquina bare metal disponíveis no  {{site.data.keyword.containerlong_notm}}.</caption>
<thead>
<th>Caso Nome e uso</th>
<th>Núcleos / Memória</th>
<th>Disco Primário / Secundário</th>
<th>Velocidade</th>
</thead>
<tbody>
<tr>
<td><strong>Bare metal com uso intensivo de RAM, mr3c.28x512</strong>: maximize a RAM disponível para os nós do trabalhador.</td>
<td>28 / 512 GB</td>
<td>2 TB de SATA/960 GB de SSD</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>GPU bare metal, mg3c.16x128</strong>: escolha esse tipo para cargas de trabalho matematicamente intensivas, como computação de alto desempenho, aprendizado de máquina ou aplicativos 3D. Esse tipo tem uma placa física Tesla K80 que tem duas unidades de processamento de gráfico (GPUs) por cartão para um total de duas GPUs.</td>
<td>16/128 GB</td>
<td>2 TB de SATA/960 GB de SSD</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>GPU bare metal, mg3c.28x256</strong>: escolha esse tipo para cargas de trabalho matematicamente intensivas, como computação de alto desempenho, aprendizado de máquina ou aplicativos 3D. Esse tipo tem duas placas físicas Tesla K80 que têm duas GPUs por cartão para um total de quatro GPUs.</td>
<td>28/256 GB</td>
<td>2 TB de SATA/960 GB de SSD</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com uso intensivo de dados, md3c.16x64.4x4tb</strong>: use esse tipo para uma quantidade significativa de armazenamento em disco local, incluindo RAID para aumentar a disponibilidade de dados, para cargas de trabalho, como sistemas de arquivos distribuídos, bancos de dados grandes e análise de big data.</td>
<td>16/64 GB</td>
<td>2x2 TB de RAID1/4x4 TB de SATA RAID10</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com uso intensivo de dados, md3c.28x512.4x4tb</strong>: use esse tipo para uma quantidade significativa de armazenamento em disco local, incluindo RAID para aumentar a disponibilidade de dados, para cargas de trabalho, como sistemas de arquivos distribuídos, bancos de dados grandes e análise de big data.</td>
<td>28 / 512 GB</td>
<td>2x2 TB de RAID1/4x4 TB de SATA RAID10</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>Bare metal balanceado, mb3c.4x32</strong>: use para cargas de trabalho balanceadas que requerem mais recursos de cálculo do que as máquinas virtuais podem oferecer. Esse tipo também pode ser ativado com o Intel® Software Guard Extensions (SGX) para que seja possível usar o <a href="/docs/services/data-shield?topic=data-shield-getting-started#getting-started" target="_blank">{{site.data.keyword.datashield_short}} (Beta)<img src="../icons/launch-glyph.svg" alt="Ícone de link externo"></a> para criptografar a sua memória de dados.</td>
<td>4/32 GB</td>
<td>2 TB SATA/2 TB SATA</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>Bare metal balanceado, mb3c.16x64</strong>: use para cargas de trabalho balanceadas que requerem mais recursos de cálculo do que as máquinas virtuais podem oferecer.</td>
<td>16/64 GB</td>
<td>2 TB de SATA/960 GB de SSD</td>
<td>10000 Mbps</td>
</tr>
<tr>
</tbody>
</table>

## Máquinas de armazenamento definido pelo software (SDS)
{: #sds}

Os tipos de software-defined storage (SDS) são máquinas físicas que são provisionadas com discos rígidos adicionais para armazenamento local físico. Diferentemente do disco local primário e secundário, esses discos rígidos não são limpos durante uma atualização ou um recarregamento do nó do trabalhador. Como os dados são colocados com o nó de cálculo, as máquinas SDS são adequadas para cargas de trabalho de alto desempenho.
{: shortdesc}

**Quando uso tipos do SDS?**</br>
Você normalmente usa máquinas SDS nos casos a seguir:
*  Se você usar um complemento SDS, como o [Portworx](/docs/containers?topic=containers-portworx#portworx) para o cluster, use uma máquina SDS.
*  Se seu app é um [StatefulSet ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/) que requer armazenamento local, é possível usar máquinas SDS e provisionar [volumes persistentes locais do Kubernetes (beta) ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/blog/2018/04/13/local-persistent-volumes-beta/).
*  Você pode ter apps customizados que requeiram armazenamento local bruto adicional.

Para obter mais soluções de armazenamento, veja [Planejando o armazenamento persistente altamente disponível](/docs/containers?topic=containers-storage_planning#storage_planning).

**Quais tipos do SDS posso solicitar?**</br>
Os tipos de nó do trabalhador variam por zona. A tabela a seguir inclui a versão mais recente de um tipo, como tipos de nós do trabalhador `x3c` do Ubuntu 18, em oposição aos tipos de nó do trabalhador `x2c` do Ubuntu 16 mais antigos. Para ver os tipos de máquina disponíveis em sua zona, execute `ibmcloud ks machine-types <zone>`. Também é possível revisar os tipos de máquina [bare metal](#bm) ou [VM](#vm) disponíveis.

Escolha um tipo de máquina com a configuração de armazenamento correta para suportar sua carga de trabalho. Alguns tipos têm uma combinação dos discos e configurações de armazenamento a seguir. Por exemplo, alguns tipos podem ter um disco primário SATA com um disco secundário SSD bruto.

* **SATA**: um dispositivo de armazenamento em disco de rotação magnética que é usado frequentemente para o disco primário do nó do trabalhador que armazena o sistema de arquivos de S.O.
* **SSD**: um dispositivo de armazenamento de unidade de estado sólido para dados de alto desempenho.
* **Bruto**: o dispositivo de armazenamento não está formatado, com a capacidade total disponível para uso.
* **RAID**: o dispositivo de armazenamento tem dados distribuídos para redundância e desempenho que variam dependendo do nível do RAID. Como tal, a capacidade do disco que está disponível para uso varia.


{: #sds-table}
<table>
<caption>Tipos de máquina SDS disponíveis no  {{site.data.keyword.containerlong_notm}}.</caption>
<thead>
<th>Caso Nome e uso</th>
<th>Núcleos / Memória</th>
<th>Disco Primário / Secundário</th>
<th>Discos brutos adicionais</th>
<th>Velocidade</th>
</thead>
<tbody>
<tr>
<td><strong>Bare metal com SDS, ms3c.4x32.1.9tb.ssd</strong>: se um armazenamento local adicional for necessário para o desempenho, use esse tipo com uso intensivo de disco, que suporta o armazenamento definido por software (SDS).</td>
<td>4/32 GB</td>
<td>2 TB de SATA/960 GB de SSD</td>
<td>1,9 TB de SSD bruto (caminho do dispositivo: `/dev/sdc`)</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com SDS, ms3c.16x64.1.9tb.ssd</strong>: se um armazenamento local adicional for necessário para o desempenho, use esse tipo com uso intensivo de disco, que suporta o armazenamento definido por software (SDS).</td>
<td>16/64 GB</td>
<td>2 TB de SATA/960 GB de SSD</td>
<td>1,9 TB de SSD bruto (caminho do dispositivo: `/dev/sdc`)</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com SDS, ms3c.28x256.3.8tb.ssd</strong>: se um armazenamento local adicional for necessário para o desempenho, use esse tipo com uso intensivo de disco, que suporta o armazenamento definido por software (SDS).</td>
<td>28/256 GB</td>
<td>2 TB de SATA/1,9 TB de SSD</td>
<td>SSD bruto de 3,8 TB (caminho do dispositivo: `/dev/sdc`)</td>
<td>10000 Mbps</td>
</tr>
<tr>
<td><strong>Bare metal com SDS, ms3c.28x512.4x3.8tb.ssd</strong>: se um armazenamento local adicional for necessário para o desempenho, use esse tipo com uso intensivo de disco, que suporta o armazenamento definido por software (SDS).</td>
<td>28 / 512 GB</td>
<td>2 TB de SATA/1,9 TB de SSD</td>
<td>4 discos, SSD bruto de 3,8 TB (caminhos do dispositivo: `/dev/sdc`, `/dev/sdd`, `/dev/sde`, `/dev/sdf`)</td>
<td>10000 Mbps</td>
</tr>
</tbody>
</table>

## Reservas de recursos do nó do trabalhador
{: #resource_limit_node}

O {{site.data.keyword.containerlong_notm}} configura as reservas de recursos de cálculo que limitam os recursos de cálculo disponíveis em cada nó do trabalhador. Os recursos de memória e CPU reservados não podem ser usados por pods no nó do trabalhador e reduzem os recursos alocáveis em cada nó do trabalhador. Quando você implementa inicialmente os pods, se o nó do trabalhador não tiver recursos alocáveis suficientes, a implementação falhará. Além disso, se os pods excederem o limite de recurso do nó do trabalhador, os pods serão despejados. No Kubernetes, esse limite é chamado de [limite máximo de despejo ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/#hard-eviction-thresholds).
{:shortdesc}

Se menos CPU ou memória estiver disponível do que as reservas do nó do trabalhador, o Kubernetes começará a despejar os pods para restaurar recursos de cálculo suficientes. Os pods serão reagendados em outro nó do trabalhador se um nó do trabalhador estiver disponível. Se os pods forem despejados frequentemente, inclua mais nós do trabalhador em seu cluster ou configure [limites de recurso ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#resource-requests-and-limits-of-pod-and-container) em seus pods.

Os recursos que são reservados em seu nó do trabalhador dependem da quantia de CPU e memória que acompanha seu nó do trabalhador. O {{site.data.keyword.containerlong_notm}} define as camadas de memória e de CPU, conforme mostrado nas tabelas a seguir. Se o nó do trabalhador vem com recursos de cálculo em múltiplas camadas, uma porcentagem de seus recursos de CPU e memória é reservada para cada camada.

Para revisar quantos recursos de cálculo são usados atualmente no nó do trabalhador, execute [`kubectl top node` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/#top).
{: tip}

<table summary="Esta tabela mostra as reservas de memória do nó do trabalhador por camada.">
<caption>Reservas de memória do nó do trabalhador por camada.</caption>
<thead>
<tr>
  <th>Camada de memória</th>
  <th>% ou quantia reservada</th>
  <th>Exemplo `b3c.4x16` do nó do trabalhador (16 GB)</th>
  <th>Exemplo do nó do trabalhador `mg1c.28x256` (256 GB)</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Primeiros 4 GB (0-4 GB)</td>
  <td>25% da memória</td>
  <td>1 GB</td>
  <td>1 GB</td>
</tr>
<tr>
  <td>Próximos 4 GB (5-8 GB)</td>
  <td>20% da memória</td>
  <td>0,8 GB</td>
  <td>0,8 GB</td>
</tr>
<tr>
  <td>Próximos 8 GB (9-16 GB)</td>
  <td>10% de memória</td>
  <td>0,8 GB</td>
  <td>0,8 GB</td>
</tr>
<tr>
  <td>Próximos 112 GB (17-128 GB)</td>
  <td>6% de memória</td>
  <td>N/A</td>
  <td>6,72 GB</td>
</tr>
<tr>
  <td>GBs restantes (129 GB+)</td>
  <td>2% de memória</td>
  <td>N/A</td>
  <td>2,54 GB</td>
</tr>
<tr>
  <td>Reserva adicional para o despejo de [`kubelet` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/administer-cluster/out-of-resource/)</td>
  <td>100 MB</td>
  <td>100 MB (quantia simples)</td>
  <td>100 MB (quantia simples)</td>
</tr>
<tr>
  <td>**Total reservado**</td>
  <td>**(varia)**</td>
  <td>**2.7 GB de um total de 16 GB**</td>
  <td>**11.96 GB de um total de 256 GB**</td>
</tr>
</tbody>
</table>

<table summary="Esta tabela mostra as reservas de CPU do nó do trabalhador por camada.">
<caption>Reservas de CPU do nó do trabalhador por camada.</caption>
<thead>
<tr>
  <th>Camada da CPU</th>
  <th>% reservada</th>
  <th>Exemplo `b3c.4x16` do nó do trabalhador (4 núcleos)</th>
  <th>Exemplo do nó do trabalhador `mg1c.28x256` (28 núcleos)</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Primeiro núcleo (núcleo 1)</td>
  <td>6% núcleos</td>
  <td>0,06 núcleos</td>
  <td>0,06 núcleos</td>
</tr>
<tr>
  <td>Próximos 2 núcleos (núcleos 2-3)</td>
  <td>1% núcleos</td>
  <td>0,02 núcleos</td>
  <td>0,02 núcleos</td>
</tr>
<tr>
  <td>Próximos 2 núcleos (núcleos 4-5)</td>
  <td>0,5% núcleos</td>
  <td>0,005 núcleos</td>
  <td>0,01 núcleos</td>
</tr>
<tr>
  <td>Núcleos restantes (Núcleos 6 +)</td>
  <td>0,25% núcleos</td>
  <td>N/A</td>
  <td>0,0575 núcleos</td>
</tr>
<tr>
  <td>**Total reservado**</td>
  <td>**(varia)**</td>
  <td>**0,085 núcleos de 4 núcleos totais**</td>
  <td>**0,1475 núcleos de 28 núcleos totais**</td>
</tr>
</tbody>
</table>
