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



# Configuration de sous-réseaux pour les clusters
{: #subnets}

Vous pouvez modifier le pool d'adresses IP publiques ou privées portables disponibles pour les services d'équilibreur de charge de réseau (NLB) en ajoutant des sous-réseaux à votre cluster Kubernetes.
{:shortdesc}



## Présentation de la mise en réseau dans {{site.data.keyword.containerlong_notm}}
{: #basics}

Comprenez les concepts de base de mise en réseau dans les clusters {{site.data.keyword.containerlong_notm}}. {{site.data.keyword.containerlong_notm}} utilise des VLAN, des sous-réseaux et des adresses IP pour fournir la connectivité réseau aux composants du cluster.
{: shortdesc}

### Réseaux locaux virtuels (VLAN)
{: #basics_vlans}

Lorsque vous créez un cluster, les noeuds worker du cluster sont connectés automatiquement à un VLAN. Un VLAN configure un groupe de noeuds worker et de pods comme s'ils étaient reliés physiquement au même câble et fournit un canal pour la connectivité entre les noeuds worker et les pods.
{: shortdesc}

<dl>
<dt>VLAN pour clusters gratuits</dt>
<dd>Dans les clusters gratuits, les noeuds worker du cluster sont connectés par défaut à un VLAN public et un VLAN privé dont IBM est le propriétaire. IBM contrôlant les VLAN, les sous-réseaux et les adresses IP, vous ne pouvez pas créer des clusters à zones multiples ni ajouter des sous-réseaux à votre cluster, et vous ne pouvez utiliser que des services NodePort pour exposer votre application.</dd>
<dt>VLAN pour clusters standard</dt>
<dd>Dans les clusters standard, la première fois que vous créez un cluster dans une zone, un VLAN public et un VLAN privé sont automatiquement mis à votre disposition dans cette zone dans votre compte d'infrastructure IBM Cloud (SoftLayer). Pour tous les autres clusters que vous créez dans cette zone, vous devez spécifier la paire de VLAN que vous souhaitez utiliser dans cette zone. Vous pouvez réutiliser le même VLAN public et le même VLAN privé que ceux que vous avez créés pour vous car les VLAN peuvent être partagés par plusieurs clusters. </br>
</br>Vous pouvez connecter vos noeuds worker à la fois à un VLAN public et au VLAN privé, ou seulement au VLAN privé. Si vous désirez connecter vos noeuds worker uniquement à un VLAN privé, vous pouvez utiliser l'ID d'un VLAN existant ou [créer un VLAN privé](/docs/cli/reference/ibmcloud?topic=cloud-cli-manage-classic-vlans#sl_vlan_create) et utiliser l'ID lors de la création du cluster.</dd></dl>

Pour voir quels sont les VLAN mis à disposition dans chaque zone de votre compte, exécutez la commande `ibmcloud ks vlans --zone <zone>.` Pour voir quels sont les VLAN sur lesquels est mis à disposition un cluster, exécutez la commande `ibmcloud ks cluster-get --cluster <cluster_name_or_ID> --showResources` et recherchez la section **Subnet VLANs**.

L'infrastructure IBM Cloud (SoftLayer) gère les VLAN qui sont mis à disposition automatiquement lorsque vous créez votre premier cluster dans une zone. Si vous laissez un VLAN inactif, par exemple en supprimant tous les noeuds worker qui s'y trouvent, l'infrastructure IBM Cloud (SoftLayer) récupère ce VLAN. Par la suite, si vous avez besoin d'un nouveau VLAN, [contactez le support {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans).

**Puis-je changer d'avis sur mes VLAN ultérieurement ?**</br>
Vous pouvez changer la configuration de vos VLAN en modifiant les pools de noeuds worker dans votre cluster. Pour plus d'informations, voir [Modification des connexions de VLAN de vos noeuds worker](/docs/containers?topic=containers-cs_network_cluster#change-vlans).

### Sous-réseaux et adresses IP
{: #basics_subnets}

En plus des noeuds worker et des pods, des sous-réseaux sont également automatiquement mis à disposition sur les VLAN. Les sous-réseaux fournissent la connectivité réseau aux composants de votre cluster en leur affectant des adresses IP.
{: shortdesc}

Les sous-réseaux suivants sont automatiquement mis à disposition sur les VLAN public et privé :

**Sous-réseaux de VLAN public**
* Le sous-réseau public principal détermine les adresses IP publiques qui sont affectées aux noeuds worker lors de la création du cluster. Plusieurs clusters figurant sur le même VLAN peuvent partager un sous-réseau public principal.
* Le sous-réseau public portable est lié à un seul cluster et fournit 8 adresses IP publiques au cluster. 3 adresses IP sont réservées aux fonctions de l'infrastructure IBM Cloud (SoftLayer). 1 adresse IP est utilisée par l'équilibreur de charge d'application (ALB) Ingress public par défaut et 4 adresses IP peuvent être utilisées pour créer des services d'équilibreur de charge de réseau public ou davantage d'ALB publics. Les adresses IP publiques portables sont des adresses IP permanentes pouvant être utilisées pour accéder aux NLB ou aux ALB via Internet. Si vous avez besoin de plus de 4 adresses IP pour les NLB ou les ALB, voir [Ajout d'adresses IP portables](/docs/containers?topic=containers-subnets#adding_ips).

**Sous-réseaux de VLAN privé**
* Le sous-réseau privé principal détermine les adresses IP privées qui sont affectées aux noeuds worker lors de la création du cluster. Plusieurs clusters figurant sur le même VLAN peuvent partager un sous-réseau privé principal.
* Le sous-réseau privé portable est lié à un seul cluster et fournit 8 adresses IP privées au cluster. 3 adresses IP sont réservées aux fonctions de l'infrastructure IBM Cloud (SoftLayer). 1 adresse IP est utilisée par l'équilibreur de charge d'application (ALB) Ingress privé par défaut et 4 adresses IP peuvent être utilisées pour créer des services d'équilibreur de charge de réseau private ou davantage d'ALB privés. Les adresses IP privées portables sont des adresses IP permanentes pouvant être utilisées pour accéder aux NLB ou aux ALB via un réseau privé. Si vous avez besoin de plus de 4 adresses IP pour les NLB ou les ALB privés, voir [Ajout d'adresses IP portables](/docs/containers?topic=containers-subnets#adding_ips).

Pour voir tous les sous-réseaux mis à disposition dans votre compte, exécutez la commande `ibmcloud ks subnets`. Pour voir les sous-réseaux portables publics et privés liés à un cluster, vous pouvez exécuter la commande `ibmcloud ks cluster-get <cluster_name_or_ID> --showResources` et recherchez la section **Subnet VLANs**.

Dans {{site.data.keyword.containerlong_notm}}, les VLAN sont limités à 40 sous-réseaux. Si vous atteignez cette limite, vérifiez d'abord si vous pouvez [réutiliser des sous-réseaux du VLAN pour créer d'autres clusters](/docs/containers?topic=containers-subnets#subnets_custom). Si vous avez besoin d'un nouveau VLAN, commandez-en un en [contactant le support {{site.data.keyword.Bluemix_notm}}](/docs/infrastructure/vlans?topic=vlans-ordering-premium-vlans#ordering-premium-vlans). Ensuite, [créez un cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_create) qui utilise ce nouveau VLAN.
{: note}

**L'adresse IP de mes noeuds worker change-t-elle ?**</br>
Une adresse IP est affectée à votre noeud worker sur les VLAN publics ou privés que votre cluster utilise. Une fois que le noeud worker est mis à disposition, les adresses IP ne changent pas. Par exemple, les adresses IP de noeud worker sont conservées au cours des opérations `reload`, `reboot` et `update`. De plus, l'adresse IP du noeud worker est utilisée pour l'identité de noeud worker dans la plupart des commandes `kubectl`. Si vous changez les VLAN que le pool worker utilise, les nouveaux noeuds worker qui sont mis à disposition dans ce pool utilisent les nouveaux VLAN pour leurs adresses IP. Les adresses IP de noeud worker existantes ne changent pas, mais vous pouvez choisir de retirer les noeuds worker qui utilisent les anciens VLAN.

### Segmentation du réseau
{: #basics_segmentation}

La segmentation du réseau décrit l'approche utilisée pour diviser un réseau en plusieurs sous-réseaux. Les applications qui s'exécutent dans un sous-réseau ne peuvent pas voir ou accéder aux applications figurant dans un autre sous-réseau. Pour plus d'informations sur les options de segmentation du réseau et leurs relations avec les VLAN, voir [cette rubrique de sécurité de cluster](/docs/containers?topic=containers-security#network_segmentation).
{: shortdesc}

Cependant, dans plusieurs situations, les composants de votre cluster doivent être autorisés à communiquer sur plusieurs VLAN privés. Par exemple, si vous souhaitez créer un cluster à zones multiples, si vous disposez de plusieurs VLAN pour un cluster ou de plusieurs sous-réseaux sur le même VLAN, les noeuds worker sur les différents sous-réseaux du même VLAN ou dans différents VLAN ne peuvent pas automatiquement communiquer entre eux. Vous devez activer une fonction de routeur virtuel (VRF) ou de spanning VLAN pour votre compte d'infrastructure IBM Cloud (SoftLayer).

**Que sont les fonctions de routeur virtuel (VRF) et de spanning VLAN ?**</br>

<dl>
<dt>[Fonction de routeur virtuel (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud)</dt>
<dd>Une fonction VRF active tous les VLAN et les sous-réseaux dans votre compte d'infrastructure pour qu'ils communiquent entre eux. De plus, une fonction VRF est requise pour autoriser vos noeuds worker et le maître à communiquer via le noeud final de service privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Notez que la fonction VRF élimine l'option Spanning VLAN de votre compte car tous les VLAN sont en mesure de communiquer sauf si vous configurez un périphérique de passerelle pour gérer le trafic.</dd>
<dt>[Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning)</dt>
<dd>Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction Spanning VLAN. Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. Notez que vous ne pouvez pas activer le noeud final de service privé si vous choisissez d'activer la fonction Spanning VLAN à la place d'une fonction VRF.</dd>
</dl>

**Comment les fonctions VRF ou Spanning VLAN affectent-elles la segmentation du réseau ?**</br>

Lorsque la fonction VRF ou Spanning VLAN est activée, tout système connecté à l'un de vos VLAN privés dans le même compte {{site.data.keyword.Bluemix_notm}} peut communiquer avec des noeuds worker. Vous pouvez isoler votre cluster des autres systèmes sur le réseau privé en appliquant des [règles de réseau privé Calico](/docs/containers?topic=containers-network_policies#isolate_workers). {{site.data.keyword.containerlong_notm}} est également compatible avec toutes les [offres de pare-feu d'infrastructure IBM Cloud (SoftLayer) ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud-computing/bluemix/network-security). Vous pouvez mettre en place un pare-feu, tel qu'un [dispositif de routeur virtuel (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra), avec des règles réseau personnalisées afin d'assurer une sécurité réseau dédiée pour votre cluster standard et détecter et parer à des intrusions réseau.

<br />



## Utilisation de sous-réseaux existants pour créer un cluster
{: #subnets_custom}

Lorsque vous créez un cluster standard, des sous-réseaux sont automatiquement créés pour vous. Néanmoins, au lieu d'utiliser ces sous-réseaux, vous pouvez utiliser des sous-réseaux portables existants dans votre compte d'infrastructure IBM Cloud (SoftLayer) ou réutiliser les sous-réseaux d'un cluster supprimé.
{:shortdesc}

Utilisez cette option pour conserver des adresses IP statiques stables lors de création ou de suppression de clusters ou pour commander des blocs d'adresses IP plus importants. Si vous souhaitez à la place obtenir plus d'adresses IP privées portables pour les services d'équilibreur de charge de réseau de votre cluster en utilisant votre propre sous-réseau de réseau sur site, voir [Ajout d'adresses IP privées portables en ajoutant des sous-réseaux gérés par l'utilisateur aux VLAN privés](#subnet_user_managed).

Les adresses IP publiques portables sont facturées au mois. Si vous retirez des adresses IP publiques portables après la mise en place de votre cluster, vous devez quand même payer les frais mensuels, même si vous ne les avez utilisées que brièvement.
{: note}

Avant de commencer :
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
- Pour réutiliser des sous-réseaux d'un cluster que vous n'utilisez plus, supprimez le cluster inutile. Créez le nouveau cluster immédiatement car les sous-réseaux sont supprimés au bout de 24 heures si vous ne les réutilisez pas.

   ```
   ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
   ```
   {: pre}

</br>Pour utiliser un sous-réseau existant dans votre portefeuille d'infrastructure IBM Cloud (SoftLayer) :

1. Procurez-vous l'ID du sous-réseau à utiliser et l'ID du VLAN sur lequel figure ce sous-réseau.

    ```
    ibmcloud ks subnets
    ```
    {: pre}

    Dans cet exemple de sortie, l'ID du sous-réseau est `1602829` et l'ID du VLAN est `2234945` :
    ```
    Getting subnet list...
    OK
    ID        Network             Gateway          VLAN ID   Type      Bound Cluster
    1550165   10.xxx.xx.xxx/26    10.xxx.xx.xxx    2234947   private
    1602829   169.xx.xxx.xxx/28   169.xx.xxx.xxx   2234945   public
    ```
    {: screen}

2. [Créez un cluster dns l'interface CLI](/docs/containers?topic=containers-clusters#clusters_cli_steps) à l'aide de l'ID de VLAN que vous avez identifié. Ajoutez l'indicateur `--no-subnet` pour empêcher la création automatique d'un nouveau sous-réseau d'adresses IP publiques portable et d'un nouveau sous-réseau d'adresses IP privées portable.

    ```
    ibmcloud ks cluster-create --zone dal10 --machine-type b3c.4x16 --no-subnet --public-vlan 2234945 --private-vlan 2234947 --workers 3 --name my_cluster
    ```
    {: pre}
    Si vous avez oublié dans quelle zone se trouve le VLAN pour l'indicateur `--zone`, vous pouvez vérifier s'il figure dans une zone particulière en exécutant la commande `ibmcloud ks vlans --zone <zone>`.
    {: tip}

3.  Vérifiez que le cluster a été créé. La commande des machines de noeud worker et la configuration et la mise à disposition du cluster dans votre compte peuvent prendre jusqu'à 15 minutes.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

    Lorsque la mise à disposition de votre cluster est terminée, la valeur de **State** passe à `deployed`.

    ```
    Name         ID                                   State      Created          Workers    Zone      Version     Resource Group Name
    mycluster    aaf97a8843a29941b49a598f516da72101   deployed   20170201162433   3          dal10     1.13.6      Default
    ```
    {: screen}

4.  Vérifiez le statut des noeuds worker.

    ```
    ibmcloud ks workers --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Avant de passer à l'étape suivante, les noeuds worker doivent être prêts. La valeur de **State** passe à `normal` et la zone **Status** affiche `Ready`.

    ```
    ID                                                  Public IP        Private IP     Machine Type   State      Status   Zone     Version
    prod-dal10-pa8dfcc5223804439c87489886dbbc9c07-w1    169.xx.xxx.xxx   10.xxx.xx.xxx  free           normal     Ready    dal10      1.13.6
    ```
    {: screen}

5.  Ajoutez le sous-réseau à votre cluster en spécifiant l'ID du sous-réseau. Lorsque vous rendez disponible un sous-réseau pour un cluster, un fichier configmap Kubernetes est créé pour vous et inclut toutes les adresses IP publiques portables disponibles que vous pouvez utiliser. S'il n'existe aucun équilibreur de charge d'application (ALB) Ingress dans la zone où se trouve le VLAN du sous-réseau, une adresse IP publique portable et une adresse IP privée portable sont automatiquement utilisées pour créer les ALB public et privé pour cette zone. Vous pouvez utiliser toutes les autres adresses publiques et privées portables de ce sous-réseau pour créer des services d'équilibreur de charge de réseau pour vos applications.

  ```
  ibmcloud ks cluster-subnet-add --cluster <cluster_name_or_id> --subnet-id <subnet_ID>
  ```
  {: pre}

  Exemple de commande :
  ```
  ibmcloud ks cluster-subnet-add --cluster mycluster --subnet-id 807861
  ```
  {: screen}

6. **Important** : pour activer la communication entre des noeuds worker qui se trouvent dans différents sous-réseaux d'un même VLAN, vous devez [activer le routage entre les sous-réseaux sur le même VLAN](#subnet-routing).

<br />


## Gestion des adresses IP portables existantes
{: #managing_ips}

Par défaut, 4 adresses IP publiques portables et 4 adresses IP privées portables peuvent être utilisées pour exposer des applications individuelles sur le réseau public ou privé en [créant un service d'équilibreur de charge de réseau (NLB)](/docs/containers?topic=containers-loadbalancer) ou en [créant des équilibreurs de charge d'application Ingress (ALB) supplémentaires](/docs/containers?topic=containers-ingress#scale_albs). Pour créer un service NLB ou ALB, vous devez avoir au moins une adresse IP portable du type approprié disponible. Vous pouvez afficher les adresses IP portables disponibles ou libérer une adresse IP portable utilisée.
{: shortdesc}

### Affichage des adresses IP publiques portables disponibles
{: #review_ip}

Pour afficher la liste de toutes les adresses IP portables utilisées et disponibles dans votre cluster, vous pouvez exécuter la commande suivante :
{: shortdesc}

```
kubectl get cm ibm-cloud-provider-vlan-ip-config -n kube-system -o yaml
```
{: pre}

Pour afficher uniquement les adresses IP publiques portables disponibles pour créer des NLB publics ou davantage d'ALB publics, vous pouvez utiliser la procédure décrite ci-après. 

Avant de commencer :
-  Vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom `default`.
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour répertorier les adresses IP publiques portables disponibles :

1.  Créez un fichier de configuration de service Kubernetes nommé `myservice.yaml` et définissez un service de type `LoadBalancer` avec une adresse IP NLB factice. L'exemple suivant utilise l'adresse IP 1.1.1.1 comme adresse IP NLB. Remplacez `<zone>` par la zone dans laquelle vous voulez rechercher des adresses IP disponibles.

    ```
    apiVersion: v1
    kind: Service
    metadata:
      labels:
        run: myservice
      name: myservice
      namespace: default
      annotations:
        service.kubernetes.io/ibm-load-balancer-cloud-provider-zone: "<zone>"
    spec:
      ports:
      - port: 80
        protocol: TCP
        targetPort: 80
      selector:
        run: myservice
      sessionAffinity: None
      type: LoadBalancer
      loadBalancerIP: 1.1.1.1
    ```
    {: codeblock}

2.  Créez le service dans votre cluster.

    ```
    kubectl apply -f myservice.yaml
    ```
    {: pre}

3.  Inspectez le service.

    ```
    kubectl describe service myservice
    ```
    {: pre}

    La création de ce service échoue car le maître Kubernetes ne parvient pas à localiser l'adresse IP NLB dans le fichier configmap Kubernetes. Lorsque vous exécutez cette commande, le message d'erreur s'affiche, ainsi que la liste des adresses IP publiques disponibles pour le cluster.

    ```
    Erreur sur l'équilibreur de charge cloud a8bfa26552e8511e7bee4324285f6a4a pour le service default/myservice portant l'UID 8bfa2655-2e85-11e7-bee4-324285f6a4af: L'adresse IP 1.1.1.1 du fournisseur cloud demandée n'est pas disponible. Les adresses IP suivantes de fournisseur cloud sont disponibles : <list_of_IP_addresses>
    ```
    {: screen}

<br />


### Libération des adresses IP utilisées
{: #free}

Vous pouvez libérer une adresse IP portable utilisée en supprimant le service d'équilibreur de charge de réseau (NLB) ou en désactivant l'équilibreur de charge d'application Ingress (ALB) qui l'utilise.
{:shortdesc}

Avant de commencer :
-  Vérifiez que vous disposez du [rôle de service {{site.data.keyword.Bluemix_notm}} IAM **Auteur** ou **Responsable**](/docs/containers?topic=containers-users#platform) pour l'espace de nom `default`.
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour supprimer un NLB ou désactiver un ALB :

1. Répertoriez les services disponibles dans votre cluster.
    ```
    kubectl get services | grep LoadBalancer
    ```
    {: pre}

2. Retirez le service NLB ou désactivez le service ALB qui utilise une adresse IP publique ou privée. 
  * Supprimez un NLB :
    ```
    kubectl delete service <service_name>
    ```
    {: pre}
  * Désactivez un ALB :
    ```
    ibmcloud ks alb-configure --albID <ALB_ID> --disable
    ```
    {: pre}

<br />


## Ajout d'adresses IP portables
{: #adding_ips}

Par défaut, 4 adresses IP publiques portables et 4 adresses IP privées portables peuvent être utilisées pour exposer des applications individuelles sur le réseau public ou privé en [créant un service d'équilibreur de charge de réseau (NLB)](/docs/containers?topic=containers-loadbalancer). Pour créer plus de 4 NLB publics ou privés, vous pouvez obtenir des adresses IP supplémentaires en ajoutant des sous-réseaux au cluster.
{: shortdesc}

Lorsque vous rendez un sous-réseau accessible à un cluster, les adresses IP de ce sous-réseau sont utilisées pour la mise en réseau du cluster. Pour éviter des conflits d'adresse IP, prenez soin d'utiliser un sous-réseau avec un seul cluster. N'utilisez pas en même temps un sous-réseau pour plusieurs clusters ou à d'autres fins hors d'{{site.data.keyword.containerlong_notm}}.
{: important}

### Ajout d'adresses IP portables en commandant d'autres sous-réseaux
{: #request}

Vous pouvez obtenir des adresses IP supplémentaires pour les services NLB en créant un nouveau sous-réseau dans un compte d'infrastructure IBM Cloud (SoftLayer) et en le rendant disponible pour le cluster que vous avez spécifié.
{:shortdesc}

Les adresses IP publiques portables sont facturées au mois. Si vous retirez des adresses IP publiques portables après la mise à disposition de votre sous-réseau, vous devez quand même payer les frais mensuels, même si vous ne les avez utilisées que brièvement.
{: note}

Avant de commencer :
-  Vérifiez que vous disposez du [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Opérateur** ou **Administrateur**](/docs/containers?topic=containers-users#platform) pour le cluster.
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

Pour commander un sous-réseau :

1. Provisionnez un nouveau sous-réseau.

    ```
    ibmcloud ks cluster-subnet-create --cluster <cluster_name_or_id> --size <subnet_size> --vlan <VLAN_ID>
    ```
    {: pre}

    <table>
    <caption>Description des composantes de cette commande</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Icône Idée"/> Description des composantes de cette commande</th>
    </thead>
    <tbody>
    <tr>
    <td><code><em>&lt;cluster_name_or_id&gt;</em></code></td>
    <td>Remplacez <code>&lt;cluster_name_or_id&gt;</code> par le nom ou l'ID du cluster.</td>
    </tr>
    <tr>
    <td><code><em>&lt;subnet_size&gt;</em></code></td>
    <td>Remplacez <code>&lt;subnet_size&gt;</code> par le nombre d'adresses IP que vous souhaitez créer dans le sous-réseau portable. Valeurs admises : 8, 16, 32 ou 64. <p class="note"> Lorsque vous ajoutez des adresses IP portables à votre sous-réseau, trois adresses IP sont utilisées pour les opérations de réseau internes au cluster. Vous ne pouvez pas utiliser ces trois adresses IP pour votre équilibreurs de charge d'application (ALB) Ingress ou pour créer des services d'équilibreur de charge de réseau (NLB). Par exemple, si vous demandez huit adresses IP publiques portables, vous pouvez en utiliser cinq pour exposer vos applications au public.</p> </td>
    </tr>
    <tr>
    <td><code><em>&lt;VLAN_ID&gt;</em></code></td>
    <td>Remplacez <code>&lt;VLAN_ID&gt;</code> par l'ID du réseau local virtuel (VLAN) privé ou public sur lequel vous désirez allouer les adresses IP publiques ou privées portables. Vous devez sélectionner un VLAN public ou privé auquel un noeud worker existant est connecté. Pour voir quels sont les VLAN public ou privé auxquels vos noeuds worker sont connectés, exécutez la commande <code>ibmcloud ks cluster-get --cluster &lt;cluster&gt; --showResources</code> et recherchez la section <strong>Subnet VLANs</strong> dans la sortie de cette commande. Le sous-réseau est fourni dans la même zone que le VLAN.</td>
    </tr>
    </tbody></table>

2. Vérifiez que le sous-réseau a bien été créé et ajouté à votre cluster. Le CIDR du sous-réseau est répertorié dans la section **Subnet VLANs**.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name_or_ID>
    ```
    {: pre}

    Dans cet exemple de sortie, un deuxième sous-réseau a été ajouté au VLAN public `2234945` :
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR          Public   User-managed
    2234947   10.xxx.xx.xxx/29     false    false
    2234945   169.xx.xxx.xxx/29    true     false
    2234945   169.xx.xxx.xxx/29    true     false
    ```
    {: screen}

3. **Important** : pour activer la communication entre des noeuds worker qui se trouvent dans différents sous-réseaux d'un même VLAN, vous devez [activer le routage entre les sous-réseaux sur le même VLAN](#subnet-routing).

<br />


### Ajout d'adresses IP privées portables en ajoutant des sous-réseaux gérés par l'utilisateur aux VLAN privés
{: #subnet_user_managed}

Vous pouvez obtenir des adresses IP privées portables supplémentaires pour les services d'équilibreur de charge de réseau (NLB) en rendant un sous-réseau de réseau sur site disponible pour votre cluster.
{:shortdesc}

Vous préférez réutiliser des sous-réseaux portables existants dans votre compte d'infrastructure IBM Cloud (SoftLayer) à la place ? Voir [Utilisation de sous-réseaux d'infrastructure IBM Cloud (SoftLayer) personnalisés ou existants pour créer un cluster](#subnets_custom).
{: tip}

Conditions requises :
- Les sous-réseaux gérés par l'utilisateur peuvent être ajoutés uniquement à des réseaux locaux virtuels (VLAN) privés.
- La limite de longueur du préfixe de sous-réseau est /24 à /30. Par exemple, `169.xx.xxx.xxx/24` indique 253 adresses IP privées utilisables, alors que `169.xx.xxx.xxx/30` indique 1 adresse IP privée utilisable.
- La première adresse IP dans le sous-réseau doit être utilisée comme passerelle du sous-réseau.

Avant de commencer :
- Configurez le routage du trafic réseau vers et depuis le sous-réseau externe.
- Confirmez que vous disposez bien d'une connectivité VPN entre la passerelle réseau du centre de données sur site et le dispositif de routeur virtuel du réseau privé ou le service VPN strongSwan qui s'exécute dans votre cluster. Pour plus d'informations, voir [Configuration de la connectivité VPN](/docs/containers?topic=containers-vpn).
-  Vérifiez que vous disposez du [rôle de plateforme {{site.data.keyword.Bluemix_notm}} IAM **Opérateur** ou **Administrateur**](/docs/containers?topic=containers-users#platform) pour le cluster.
- [Connectez-vous à votre compte. Le cas échéant, ciblez le groupe de ressources approprié. Définissez le contexte pour votre cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)


Pour ajouter un sous-réseau à partir d'un réseau sur site :

1. Affichez l'ID du VLAN privé de votre cluster. Localisez la section **Subnet VLANs**. Dans la zone **User-managed**, identifiez l'ID VLAN avec la valeur _false_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    ```
    Subnet VLANs
  VLAN ID   Subnet CIDR         Public   User-managed
  2234947   10.xxx.xx.xxx/29    false    false
  2234945   169.xx.xxx.xxx/29  true    false
    ```
    {: screen}

2. Ajoutez le sous-réseau externe à votre VLAN privé. Les adresses IP privées portables sont ajoutées à la mappe de configuration (configmap) du cluster.

    ```
    ibmcloud ks cluster-user-subnet-add --cluster <cluster_name> --subnet-cidr <subnet_CIDR> --private-vlan <private_VLAN>
    ```
    {: pre}

    Exemple :

    ```
    ibmcloud ks cluster-user-subnet-add --cluster mycluster --subnet-cidr 10.xxx.xx.xxx/24 --private-vlan 2234947
    ```
    {: pre}

3. Vérifiez que le sous-réseau fourni par l'utilisateur a été ajouté. La zone **User-managed** a la valeur _true_.

    ```
    ibmcloud ks cluster-get --showResources <cluster_name>
    ```
    {: pre}

    Dans cet exemple de sortie, un deuxième sous-réseau a été ajouté au VLAN privé `2234947` :
    ```
    Subnet VLANs
    VLAN ID   Subnet CIDR       Public   User-managed
    2234947   10.xxx.xx.xxx/29  false    false
    2234945   169.xx.xxx.xxx/29 true     false
    2234947   10.xxx.xx.xxx/24  false    true
    ```
    {: screen}

4. [Activez le routage entre les sous-réseaux sur le même VLAN](#subnet-routing).

5. Ajoutez un [service d'équilibreur de charge de réseau (NLB) privé](/docs/containers?topic=containers-loadbalancer) ou activez un [service ALB Ingress privé](/docs/containers?topic=containers-ingress#private_ingress) pour accéder à votre application via le réseau privé. Pour utiliser une adresse IP privée du sous-réseau que vous avez ajouté, vous devez indiquer une adresse IP figurant dans le CIDR du sous-réseau. Autrement, une adresse IP est sélectionnée de manière aléatoire dans les sous-réseaux d'infrastructure IBM Cloud (SoftLayer) ou dans les sous-réseaux fournis par l'utilisateur sur le VLAN privé.

<br />


## Gestion du routage de sous-réseaux
{: #subnet-routing}

Si vous disposez de plusieurs VLAN pour un cluster, de plusieurs sous-réseaux sur le même VLAN ou d'un cluster à zones multiples, vous devez activer une fonction [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) pour votre compte d'infrastructure IBM Cloud (SoftLayer) pour que vos noeuds worker puissent communiquer entre eux sur le réseau privé. Pour activer la fonction VRF, [contactez le représentant de votre compte d'infrastructure IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Si vous ne parvenez pas à activer la fonction VRF ou si vous ne souhaitez pas le faire, activez la fonction [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Pour effectuer cette action, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`. 

Passez en revue les scénarios suivants nécessitant également la fonction Spanning VLAN.

### Activation du routage entre les sous-réseaux principaux sur le même VLAN
{: #vlan-spanning}

Lorsque vous créez un cluster, des sous-réseaux public et privé principaux sont mis à disposition sur les VLAN public et privé. Le sous-réseau public principal se termine par `/28` et fournit 14 adresses IP publiques pour vos noeuds worker. Le sous-réseau privé principal se termine par `/26` et fournit jusqu'à 62 adresses IP privées pour les noeuds worker.
{:shortdesc}

Vous pouvez dépasser les 14 adresses IP publiques et les 62 adresses IP privées initiales correspondant à vos noeuds worker en disposant d'un cluster volumineux ou de plusieurs clusters de taille plus petite au même emplacement sur le même VLAN. Lorsqu'un sous-réseau public ou privé atteint le nombre limite de noeuds worker, un autre sous-réseau principal est commandé dans le même VLAN.

Pour garantir que les noeuds worker de ces sous-réseaux principaux situés sur le même VLAN peuvent communiquer, vous devez activer la fonction Spanning VLAN. Pour obtenir les instructions correspondantes, voir [Activer ou désactiver le spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning).

Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
{: tip}

### Gestion du routage de sous-réseaux pour les périphériques de passerelle
{: #vra-routing}

Lorsque vous créez un cluster, un sous-réseau portable public et un sous-réseau portable privé sont commandés sur les VLAN auxquels est connecté le cluster. Ces sous-réseaux fournissent les adresses IP des services d'équilibreur de charge d'application (ALB) Ingress et d'équilibreur de charge de réseau (NLB).
{: shortdesc}

Cependant, si vous disposez déjà d'un dispositif de routage, tel qu'un [dispositif de routeur virtuel (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra#about-the-vra), les sous-réseaux portables récemment ajoutés des VLAN auxquels est connecté le cluster, ne sont pas configurés sur le routeur. Pour utiliser des services NLB et ALB Ingresss, vous devez vous assurer que les dispositifs réseau peuvent effectuer le routage entre les sous-réseaux sur le même VLAN en [activant la fonction Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning).

Pour vérifier si le spanning VLAN est déjà activé, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get --region <region>`.
{: tip}
