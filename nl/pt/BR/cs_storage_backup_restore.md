---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-21"

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



# Fazendo backup e restaurando dados em volumes persistentes
{: #backup_restore}

Os compartilhamentos de arquivo e o armazenamento de bloco são provisionados para a mesma zona que seu cluster. O armazenamento é hospedado em servidores em cluster pela {{site.data.keyword.IBM_notm}} para fornecer disponibilidade no caso de um servidor ficar inativo. No entanto, os compartilhamentos de arquivo e o armazenamento de bloco não serão submetidos a backup automaticamente e poderão ser inacessíveis se a zona inteira falhar. Para proteger seus dados contra perda ou danos, será possível configurar backups periódicos que poderão ser usados para restaurar seus dados quando necessário.
{: shortdesc}

Revise as opções de backup e restauração a seguir para compartilhamentos de arquivo e armazenamento de bloco do NFS:

<dl>
  <dt>Configurar capturas instantâneas periódicas</dt>
  <dd><p>É possível configurar capturas instantâneas periódicas para seu compartilhamento de arquivo ou armazenamento de bloco do NFS, que é uma imagem somente leitura que captura o estado da instância em um momento. Para armazenar a captura instantânea, deve-se solicitar espaço de captura instantânea em seu compartilhamento de arquivo ou armazenamento de bloco do NFS. As capturas instantâneas são armazenadas na instância de armazenamento existente dentro da mesma zona. É possível restaurar dados de uma captura instantânea se um usuário acidentalmente remove dados importantes do volume. </br></br> <strong>Para criar uma captura instantânea para seu volume: </strong><ol><li>PVs de Lista existente em seu cluster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Obtenha os detalhes para o PV para o qual você deseja criar espaço de captura instantânea e anote o ID do volume, o tamanho e os IOPS. <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> Para armazenamento de arquivo, o ID do volume, o tamanho e o IOPS podem ser localizados na seção <strong>Labels</strong> de sua saída da CLI. Para armazenamento de bloco, o tamanho e o IOPS são mostrados na seção <strong>Labels</strong> de sua saída da CLI. Para localizar o ID do volume, revise a anotação <code>ibm.io/network-storage-id</code> de sua saída da CLI. </li><li>Crie o tamanho da captura instantânea para o volume existente com os parâmetros que você recuperou na etapa anterior. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>Espere o tamanho da captura instantânea para criar. <pre class="pre"><code>slcli arquivo volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>O tamanho da captura instantânea é provisionado com êxito quando o <strong>Snapshot Capacity (GB)</strong> em sua saída da CLI muda de 0 para o tamanho que você pediu. </li><li>Crie a captura instantânea para o volume e anote o ID da captura instantânea que é criado para você. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>Verifique se a captura instantânea foi criada com êxito. <pre class="pre"><code>slcli arquivo volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>Para restaurar dados por meio de uma captura instantânea para um volume existente: </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>Para obter informações adicionais, veja:<ul><li>[ Capturas instantâneas periódicas do NFS ](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[Bloquear capturas instantâneas periódicas](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>Replicar capturas instantâneas para outra zona</dt>
 <dd><p>Para proteger seus dados de uma falha de zona, é possível [replicar capturas instantâneas](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication) para um compartilhamento de arquivo NFS ou uma instância de armazenamento de bloco que está configurada em outra zona. Os dados podem ser replicados do armazenamento primário para o armazenamento de backup somente. Não é possível montar uma instância replicada de compartilhamento de arquivo ou de armazenamento de bloco do NFS em um cluster. Quando seu armazenamento primário falha, é possível configurar manualmente o armazenamento de backup replicado para ser o primário. Em seguida, é possível montá-lo para seu cluster. Depois que o armazenamento primário é restaurado, é possível restaurar os dados do armazenamento de backup.</p>
 <p>Para obter informações adicionais, veja:<ul><li>[Replicar capturas instantâneas para NFS](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[Replicar capturas instantâneas para Bloco](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>Armazenamento duplicado</dt>
 <dd><p>É possível duplicar o compartilhamento de arquivo NFS ou a instância de armazenamento de bloco na mesma zona que a instância de armazenamento original. Uma duplicata tem os mesmos dados que a instância de armazenamento original no momento em que é criada. Diferentemente de réplicas, use a duplicata como uma instância de armazenamento independente da original. Para duplicar, primeiramente configure capturas instantâneas para o volume.</p>
 <p>Para obter informações adicionais, veja:<ul><li>[Capturas instantâneas de duplicata NFS](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[Capturas instantâneas de duplicata de bloco](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>Backup de dados para Object Storage</dt>
  <dd><p>É possível usar a [**imagem ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) para ativar um backup e restaurar o pod em seu cluster. Esse pod contém um script para executar um backup único ou periódico para qualquer persistent volume claim (PVC) em seu cluster. Os dados são armazenados em sua instância do {{site.data.keyword.objectstoragefull}} que você configurou em uma zona.</p>
  <p>Para tornar os seus dados ainda mais altamente disponíveis e proteger o seu app de uma falha de zona, configure uma segunda instância do {{site.data.keyword.objectstoragefull}} e replique dados entre as zonas. Se você precisa restaurar dados de sua instância do {{site.data.keyword.objectstoragefull}}, use o script de restauração que é fornecido com a imagem.</p></dd>
<dt>Copiar dados de e para pods e contêineres</dt>
<dd><p>É possível usar o [comando `kubectl cp` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) para copiar arquivos e diretórios de pods ou contêineres específicos ou para eles em seu cluster.</p>
<p>Antes de iniciar: [Efetue login em sua conta. Destine a região apropriada e, se aplicável, o grupo de recursos. Configure o contexto para seu cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure). Se você não especificar um contêiner com <code>-c</code>, o comando usará o primeiro contêiner disponível no pod.</p>
<p>É possível usar o comando de várias maneiras:</p>
<ul>
<li>Copiar dados de sua máquina local para um pod no cluster: <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copiar dados de um pod em seu cluster para a máquina local: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copiar dados de um pod em seu cluster para um contêiner específico em outro pod: <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
