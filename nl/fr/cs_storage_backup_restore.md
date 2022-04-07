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



# Sauvegarde et restauration de données dans les volumes persistants
{: #backup_restore}

Les partages de fichiers et le stockage par blocs sont mis à disposition dans la même zone que votre cluster. Le stockage est hébergé sur des serveurs en cluster par {{site.data.keyword.IBM_notm}} pour assurer la disponibilité en cas de panne de serveur. Cependant, les partages de fichiers et le stockage par blocs ne sont pas sauvegardés automatiquement et risquent d'être inaccessibles en cas de défaillance totale de la zone. Pour éviter que vos données soient perdues ou endommagées, vous pouvez configurer des sauvegardes régulières que vous pourrez utiliser pour récupérer vos données si nécessaire.
{: shortdesc}

Passez en revue les options de sauvegarde et restauration suivantes pour vos partages de fichiers NFS et votre stockage par blocs :

<dl>
  <dt>Configurer la prise d'instantanés régulière</dt>
  <dd><p>Vous pouvez configurer la prise d'instantanés régulière de votre partage de fichiers NFS ou de votre stockage par blocs. Un instantané est une image en lecture seule qui capture l'état de l'instance à un moment donné. Pour stocker l'instantané, vous devez demander de l'espace d'instantané dans votre partage de fichiers NFS ou de stockage par blocs. Les instantanés sont stockés dans l'instance de stockage existante figurant dans la même zone. Vous pouvez restaurer des données à partir d'un instantané si l'utilisateur supprime accidentellement des données importantes du volume. </br></br> <strong>Pour créer un instantané de votre volume :</strong><ol><li>Répertoriez les volumes persistants (PV) existants dans votre cluster. <pre class="pre"><code>kubectl get pv</code></pre></li><li>Obtenez les détails du volume persistant pour lequel vous voulez créer un espace d'instantané et notez l'ID du volume, la taille et le nombre d'entrées-sorties par seconde (IOPS). <pre class="pre"><code>kubectl describe pv &lt;pv_name&gt;</code></pre> Pour le stockage de fichiers, l'ID du volume, la taille et le nombre d'IOPS se trouvent dans la section <strong>Labels</strong> dans la sortie de l'interface CLI. Pour le stockage par blocs, la taille et le nombre d'IOPS sont affichés dans la section <strong>Labels</strong> de la sortie de l'interface CLI. Pour trouver l'ID du volume, consultez l'annotation <code>ibm.io/network-storage-id</code> dans la sortie de l'interface CLI. </li><li>Créez la taille de l'instantané pour le volume existant avec les paramètres que vous avez récupérés à l'étape précédente. <pre class="pre"><code>slcli file snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-order --capacity &lt;size&gt; --tier &lt;iops&gt; &lt;volume_id&gt;</code></pre></li><li>Attendez que la taille de l'instantané soit créée. <pre class="pre"><code>slcli file volume-detail &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;volume_id&gt;</code></pre>La taille de l'instantané est mise à disposition lorsque la section <strong>Snapshot Capacity (GB)</strong> de la sortie de l'interface CLI passe de 0 à la taille que vous avez commandée. </li><li>Créez l'instantané de votre volume et notez l'ID de l'instantané qui a été créé pour vous. <pre class="pre"><code>slcli file snapshot-create &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-create &lt;volume_id&gt;</code></pre></li><li>Vérifiez que la création de l'instantané a abouti. <pre class="pre"><code>slcli file volume-detail &lt;snapshot_id&gt;</code></pre><pre class="pre"><code>slcli block volume-detail &lt;snapshot_id&gt;</code></pre></li></ol></br><strong>Pour restaurer les données d'un instantané sur un volume existant : </strong><pre class="pre"><code>slcli file snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre><pre class="pre"><code>slcli block snapshot-restore -s &lt;snapshot_id&gt; &lt;volume_id&gt;</code></pre></br>Pour plus d'informations, voir :<ul><li>[Capture régulière d'instantanés NFS](/docs/infrastructure/FileStorage?topic=FileStorage-snapshots)</li><li>[Capture régulière d'instantanés de stockage par blocs](/docs/infrastructure/BlockStorage?topic=BlockStorage-snapshots#snapshots)</li></ul></p></dd>
  <dt>Répliquer les instantanés dans une autre zone</dt>
 <dd><p>Pour protéger vos données en cas de défaillance d'une zone, vous pouvez [répliquer des instantanés](/docs/infrastructure/FileStorage?topic=FileStorage-replication#replication) sur une instance de partage de fichiers NFS ou de stockage par blocs configurée dans une autre zone. Les données peuvent être répliquées du stockage principal uniquement vers le stockage de sauvegarde. Vous ne pouvez pas monter une instance de partage de fichiers NFS ou de stockage par blocs répliquée sur un cluster. En cas de défaillance de votre stockage principal, vous pouvez manuellement définir votre stockage de sauvegarde répliqué comme stockage principal. Vous pouvez ensuite le monter sur votre cluster. Une fois votre stockage principal restauré, vous pouvez récupérer les données dans le stockage de sauvegarde.</p>
 <p>Pour plus d'informations, voir :<ul><li>[Réplication d'instantanés pour NFS](/docs/infrastructure/FileStorage?topic=FileStorage-replication)</li><li>[Réplication d'instantanés de stockage par blocs](/docs/infrastructure/BlockStorage?topic=BlockStorage-replication#replication)</li></ul></p></dd>
 <dt>Dupliquer le stockage</dt>
 <dd><p>Vous pouvez dupliquer votre instance de partage de fichiers NFS ou de stockage par blocs dans la même zone que l'instance de stockage d'origine. Un doublon contient les même données que l'instance de stockage d'origine au moment où vous créez le doublon. Contrairement aux répliques, le doublon s'utilise comme une instance de stockage indépendante de l'original. Pour effectuer la duplication, configurez d'abord des instantanés pour le volume.</p>
 <p>Pour plus d'informations, voir :<ul><li>[Duplication d'instantanés NFS](/docs/infrastructure/FileStorage?topic=FileStorage-duplicatevolume#duplicatevolume)</li><li>[Duplication d'instantanés de stockage par blocs](/docs/infrastructure/BlockStorage?topic=BlockStorage-duplicatevolume#duplicatevolume)</li></ul></p></dd>
  <dt>Sauvegarder les données dans Object Storage</dt>
  <dd><p>Vous pouvez utiliser l'[**image ibm-backup-restore**](/docs/services/RegistryImages/ibm-backup-restore?topic=RegistryImages-ibmbackup_restore_starter#ibmbackup_restore_starter) pour constituer un pod de sauvegarde et de restauration dans votre cluster. Ce pod contient un script pour exécuter une sauvegarde unique ou régulière d'une réservation de volume persistant (PVC) dans votre cluster. Les données sont stockées dans votre instance {{site.data.keyword.objectstoragefull}} que vous avez configurée dans une zone.</p>
  <p>Pour rendre vos données hautement disponibles et protéger votre application en cas de défaillance d'une zone, configurez une deuxième instance {{site.data.keyword.objectstoragefull}} et répliquez les données entre les différentes zones. Si vous devez restaurer des données à partir de votre instance {{site.data.keyword.objectstoragefull}}, utilisez le script de restauration fourni avec l'image.</p></dd>
<dt>Copier les données depuis et vers des pods et des conteneurs</dt>
<dd><p>Vous pouvez utiliser la [commande![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://kubernetes.io/docs/reference/kubectl/overview/#cp) `kubectl cp` pour copier des fichiers et des répertoires depuis et vers des pods ou des conteneurs spécifiques dans votre cluster.</p>
<p>Avant de commencer : [connectez-vous à votre compte. Ciblez la région appropriée et, le cas échéant, le groupe de ressources. Définissez le contexte de votre cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure). Si vous n'indiquez pas de conteneur avec <code>-c</code>, la commande utilise le premier conteneur disponible dans le pod.</p>
<p>Vous pouvez utiliser la commande de plusieurs manières :</p>
<ul>
<li>Copier les données de votre machine locale vers un pod dans votre cluster : <pre class="pre"><code>kubectl cp <var>&lt;local_filepath&gt;/&lt;filename&gt;</var> <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var></code></pre></li>
<li>Copier les données d'un pod de votre cluster vers votre machine locale : <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;/&lt;filename&gt;</var> <var>&lt;local_filepath&gt;/&lt;filename&gt;</var></code></pre></li>
<li>Copier les données d'un pod de votre cluster vers un conteneur spécifique dans un autre pod : <pre class="pre"><code>kubectl cp <var>&lt;namespace&gt;/&lt;pod&gt;:&lt;pod_filepath&gt;</var> <var>&lt;namespace&gt;/&lt;other_pod&gt;:&lt;pod_filepath&gt;</var> -c <var>&lt;container></var></code></pre></li>
</ul></dd>
  </dl>
