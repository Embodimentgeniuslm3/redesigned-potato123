---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

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


# Planification d'une configuration de réseau pour votre cluster
{: #plan_clusters}

Concevez une configuration de réseau pour votre cluster {{site.data.keyword.containerlong}} répondant à vos besoins en termes de charges de travail et d'environnement.
{: shortdesc}

Dans un cluster {{site.data.keyword.containerlong_notm}}, vos applications conteneurisées sont hébergées sur des hôtes de calcul nommés noeuds worker. Les noeuds worker sont gérés par le maître Kubernetes. La configuration de la communication entre les noeuds worker et le maître Kubernetes, d'autres services, Internet ou d'autres réseaux privés dépend de la manière dont vous avez configuré votre réseau d'infrastructure IBM Cloud (SoftLayer).


Vous créez un cluster pour la première fois ? Exécutez d'abord notre [tutoriel](/docs/containers?topic=containers-cs_cluster_tutorial) et revenez ici lorsque vous êtes  prêt à planifier vos clusters prêts pour la production.
{: tip}

Avant de planifier la configuration de réseau de votre cluster, vous devez d'abord vous reporter à la [description des principes de base d'un réseau de cluster](#plan_basics). Vous pouvez ensuite passer en revue trois configurations de réseau de cluster potentielles qui sont adaptées à des scénarios basés sur des environnements, à savoir [Exécution de charges de travail d'application accessible sur Internet](#internet-facing), [Extension d'un centre de données sur site avec un accès public limité](#limited-public) et [Extension d'un centre de données sur site sur le réseau privé uniquement](#private_clusters).

## Description des principes de base d'un réseau de cluster
{: #plan_basics}

Lorsque vous créez votre cluster, vous devez choisir une configuration de réseau pour permettre à certains composants du cluster de communiquer entre eux et avec des réseaux et des services qui se trouvent en dehors du cluster.
{: shortdesc}

* [Communication entre les noeuds worker](#worker-worker) : Tous les noeuds worker doivent pouvoir communiquer entre eux sur le réseau privé. Dans de nombreux cas, la communication doit être autorisée sur plusieurs VLAN privés pour permettre aux noeuds worker sur différents VLAN et dans différentes zones de se connecter entre eux. 
* [Communication entre les noeuds worker et le maître et entre les utilisateurs et le maître](#workeruser-master) : Vos noeuds worker et vos utilisateurs de cluster autorisés peuvent communiquer avec le maître Kubernetes de façon sécurisée sur le réseau public  à l'aide de TLS ou sur le réseau privé via des noeuds finaux de service privé. 
* [Communication entre les noeuds worker et d'autres services {{site.data.keyword.Bluemix_notm}} ou des réseaux sur site](#worker-services-onprem) : Autorisez vos noeuds worker à communiquer de façon sécurisée avec d'autres services {{site.data.keyword.Bluemix_notm}}, tels que {{site.data.keyword.registrylong}}, et avec un réseau sur site. 
* [Communication externe avec des applications qui s'exécutent sur des noeuds worker](#external-workers) : Autorisez des demandes publiques ou privées dans le cluster ainsi que des demandes depuis le cluster vers un noeud final public. 

### Communication entre noeuds worker
{: #worker-worker}

Lorsque vous créez un cluster, les noeuds worker du cluster sont connectés automatiquement à un VLAN privé et éventuellement à un VLAN public. Un VLAN configure un groupe de noeuds worker et de pods comme s'ils étaient reliés physiquement au même câble physique et fournit un canal pour la connectivité entre les noeuds worker.
{: shortdesc}

**Connexions VLAN pour les noeuds worker**</br>
Tous les noeuds worker doivent être connectés à un VLAN privé de sorte que chaque noeud worker puisse envoyer à et recevoir des informations d'autres noeuds worker. Lorsque vous créez un cluster avec des noeuds worker qui sont également connectés à un VLAN public, vos noeuds worker peuvent communiquer automatiquement avec le maître Kubernetes via le VLAN public et via le VLAN privé si vous activez le noeud final de service privé. Le VLAN public fournit également la connectivité de réseau public de manière à vous permettre d'exposer des applications dans votre cluster sur Internet. Toutefois, si vous devez sécuriser vos applications à partir de l'interface publique, plusieurs options sont disponibles pour sécuriser votre cluster, par exemple, utiliser des règles réseau Calico ou isoler la charge de travail de réseau externe sur des noeuds worker de périphérie.
* Clusters gratuits : Dans les clusters gratuits, les noeuds worker du cluster sont connectés par défaut à un VLAN public et un VLAN privé dont IBM est le propriétaire. IBM contrôlant les VLAN, les sous-réseaux et les adresses IP, vous ne pouvez pas créer des clusters à zones multiples ni ajouter des sous-réseaux à votre cluster, et vous ne pouvez utiliser que des services NodePort pour exposer votre application.</dd>
* Clusters standard : Dans les clusters standard, la première fois que vous créez un cluster dans une zone, un VLAN public et un VLAN privé dans cette zone sont automatiquement mis à votre disposition dans votre compte d'infrastructure IBM Cloud (SoftLayer). Si vous spécifiez que des noeuds worker doivent être connectés à un VLAN privé uniquement, un VLAN privé uniquement est mis à disposition automatiquement. Pour tous les autres clusters que vous créez dans cette zone, vous pouvez spécifier la paire de VLAN que vous souhaitez utiliser. Vous pouvez réutiliser le même VLAN public et le même VLAN privé que ceux que vous avez créés pour vous car les VLAN peuvent être partagés par plusieurs clusters. 

Pour plus d'informations sur les VLAN, les sous-réseaux et les adresses IP, voir [Présentation de la mise en réseau dans {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-subnets#basics).

**Communication des noeuds worker sur des sous-réseaux et des VLAN**</br>
Dans de nombreuses situations, les composants présents dans votre cluster doivent être autorisés à communiquer sur plusieurs VLAN privés. Par exemple, si vous souhaitez créer un cluster à zones multiples, si vous disposez de plusieurs VLAN pour un cluster ou de plusieurs sous-réseaux sur le même VLAN, les noeuds worker sur les différents sous-réseaux du même VLAN ou dans différents VLAN ne peuvent pas automatiquement communiquer entre eux. Vous devez activer la fonction de routeur virtuel (VRF) ou de spanning VLAN pour votre compte d'infrastructure IBM Cloud (SoftLayer).

* [Virtual Routing and Forwarding (VRF)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud) : Une fonction VRF active tous les VLAN et les sous-réseaux dans votre compte d'infrastructure pour qu'ils communiquent entre eux. De plus, une fonction VRF est requise pour autoriser vos noeuds worker et le maître à communiquer via le noeud final de service privé et pour communiquer avec d'autres instances {{site.data.keyword.Bluemix_notm}} qui prennent en charge les noeuds finaux de service privé. Pour activer VRF, exécutez la commande `ibmcloud account update --service-endpoint-enable true`. Le résultat de cette commande vous invite à ouvrir un cas de support pour permettre à votre compte d'utiliser une fonction VRF et des points finaux de service. La fonction VRF élimine l'option Spanning VLAN de votre compte car tous les VLAN sont en mesure de communiquer.</br></br>Lorsque la fonction VRF est activée, tout système qui est connecté à l'un de vos VLAN privés dans le même compte {{site.data.keyword.Bluemix_notm}} peut communiquer avec les noeuds worker du cluster. Vous pouvez isoler votre cluster des autres systèmes sur le réseau privé en appliquant des [règles de réseau privé Calico](/docs/containers?topic=containers-network_policies#isolate_workers).</dd>
* [Spanning VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning) : Si vous ne pouvez ou souhaitez pas activer la fonction VRF, par exemple, si vous n'avez pas besoin que le maître soit accessible sur le réseau privé ou si vous utilisez un périphérique de passerelle pour accéder au maître via le VLAN public, activez la fonction Spanning VLAN. Par exemple, si vous avez un périphérique de passerelle et que vous ajoutez un cluster, les nouveaux sous-réseaux portables qui sont commandés pour le cluster ne sont pas configurés sur ce périphérique de passerelle mais la fonction Spanning VLAN active le routage entre les sous-réseaux. Pour activer la fonction Spanning VLAN, vous devez disposer du [droit d'infrastructure](/docs/containers?topic=containers-users#infra_access) **Réseau > Gérer le spanning VLAN pour réseau**, ou vous pouvez demander au propriétaire du compte de l'activer. Pour vérifier si la fonction Spanning VLAN est déjà activée, utilisez la [commande](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`. Vous ne pouvez pas activer le noeud final de service privé si vous choisissez d'activer la fonction Spanning VLAN à la place d'une fonction VRF.

</br>

### Communication entre les noeuds worker et le maître et entre les utilisateurs et le maître
{: #workeruser-master}

Un canal de communication doit être configuré pour que les noeuds worker puissent établir une connexion au maître Kubernetes. Vous pouvez permettre la communication entre vos noeuds worker et le maître Kubernetes en activant le noeud final de service public uniquement, des noeuds finaux de service public et privé ou le noeud final de service privé uniquement.
{: shortdesc}

Pour sécuriser la communication via des noeuds finaux de service public et privé, {{site.data.keyword.containerlong_notm}} configure automatiquement une connexion OpenVPN entre le maître Kubernetes et le noeud worker lors de la création du cluster. Les noeuds worker dialoguent en toute sécurité avec le maître via des certificats TLS et le maître dialogue avec eux via une connexion OpenVPN.

**Noeud final de service public uniquement**</br>
Si vous ne souhaitez ou ne pouvez pas activer VRF pour votre compte, vos noeuds worker peuvent automatiquement se connecter au maître Kubernetes sur le VLAN public via le noeud final de service public. 
* La communication entre les noeuds worker et le maître est établie de manière sécurisée sur le réseau public via le noeud final de service public. 
* Le maître est accessible au public pour les utilisateurs de cluster autorisés uniquement via le noeud final de service public. Les utilisateurs de votre cluster peuvent accéder de manière sécurisée à votre maître Kubernetes sur Internet pour exécuter des commandes `kubectl`, par exemple.

**Noeuds finaux de service public et privé**</br>
Pour rendre votre maître accessible au public ou en privé aux utilisateurs du cluster, vous pouvez activer les noeuds finaux de service public et privé. La fonction VRF est requise dans votre compte {{site.data.keyword.Bluemix_notm}} et vous devez activer votre compte pour l'utilisation de noeuds finaux de service. Pour activer la fonction VRF et des noeuds finaux de service, exécutez `ibmcloud account update --service-endpoint-enable true`.
* Si des noeuds worker sont connectés aux VLAN publics et privés, la communication entre les noeuds worker et le maître est établie sur le réseau privé via le noeud final de service privé et sur le réseau public via le noeud final de service public. En acheminant la moitié du trafic worker vers maître via le noeud final public et l'autre moitié via le noeud final privé, votre communication maître vers worker est protégée contre d'éventuelles pannes du réseau public ou privé. Si des noeuds worker sont connectés aux VLAN privés uniquement, la communication entre les noeuds worker et le maître est établie sur le réseau privé via le noeud final de service privé uniquement. 
* Le maître est accessible au public pour les utilisateurs de cluster autorisés via le noeud final de service public. Le maître est accessible en privé via le noeud final de service privé si les utilisateurs de cluster autorisés se trouvent dans votre réseau privé {{site.data.keyword.Bluemix_notm}} ou s'ils sont connectés au réseau privé via une connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link. Notez que vous devez [exposer le noeud final maître via un équilibreur de charge privé](/docs/containers?topic=containers-clusters#access_on_prem) de sorte que les utilisateurs puissent accéder au maître via une connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link.


**Noeud final de service privé uniquement**</br>
Pour rendre votre maître accessible uniquement en privé, vous pouvez activer le noeud final de service privé. La fonction VRF est requise dans votre compte {{site.data.keyword.Bluemix_notm}} et vous devez activer votre compte pour l'utilisation de noeuds finaux de service. Pour activer la fonction VRF et des noeuds finaux de service, exécutez `ibmcloud account update --service-endpoint-enable true`. Notez que l'utilisation de noeud final de service privé entraîne des frais de bande passante non facturés ou non mesurés. 
* La communication entre les noeuds worker et le maître est établie sur le réseau privé via le noeud final de service privé. 
* Le maître est accessible en privé si les utilisateurs de cluster autorisés figurent dans votre réseau privé {{site.data.keyword.Bluemix_notm}} ou s'ils sont connectés au réseau privé via une connexion VPN ou Direct Link. Notez que vous devez [exposer le noeud final maître via un équilibreur de charge privé](/docs/containers?topic=containers-clusters#access_on_prem) de sorte que les utilisateurs puissent accéder au maître via une connexion VPN ou Direct Link.


</br>

### Communication entre les noeuds Worker et d'autres services {{site.data.keyword.Bluemix_notm}} ou réseaux sur site
{: #worker-services-onprem}

Autorisez vos noeuds worker à communiquer de façon sécurisée avec d'autres services {{site.data.keyword.Bluemix_notm}}, tels que {{site.data.keyword.registrylong}}, et avec un réseau sur site.
{: shortdesc}

**Communication avec d'autres services {{site.data.keyword.Bluemix_notm}} sur le réseau privé ou public**</br>
Vos noeuds worker peuvent communiquer automatiquement et en toute sécurité avec d'autres services {{site.data.keyword.Bluemix_notm}} qui prennent en charge des noeuds finaux de service privé, par exemple, {{site.data.keyword.registrylong}}, sur votre réseau privé d'infrastructure IBM Cloud (SoftLayer). Si un service {{site.data.keyword.Bluemix_notm}} ne prend pas en charge des noeuds finaux de service privé, vos noeuds worker doivent être connectés à un VLAN public de manière à pouvoir communiquer en toute sécurité avec les services sur le réseau public. 

Si vous utilisez des règles réseau Calico pour verrouiller le réseau public dans votre cluster, vous devrez peut-être autoriser l'accès aux adresses IP publiques et privées des services que vous souhaitez utiliser dans vos règles Calico. Si vous utilisez un périphérique de passerelle, par exemple, un dispositif Vyatta (Virtual Router Appliance), vous devez [autoriser l'accès aux adresses IP privées des services que vous souhaitez utiliser](/docs/containers?topic=containers-firewall#firewall_outbound) dans votre pare-feu de périphérique de passerelle.
{: note}

**{{site.data.keyword.BluDirectLink}} pour la communication sur le réseau privé avec des ressources figurant dans des centres de données sur site**</br>
Pour connecter votre cluster à votre centre de données sur site, par exemple avec {{site.data.keyword.icpfull_notm}}, vous pouvez configurer [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Avec {{site.data.keyword.Bluemix_notm}} Direct Link, vous créez une connexion privée directe entre vos environnements de réseau distants et {{site.data.keyword.containerlong_notm}} sans routage sur l'internet public. 

**Connexion VPN IPSec strongSwan pour la communication sur le réseau public avec des ressources figurant dans des centres de données sur site**
* Noeuds worker qui sont connectés à des VLAN publics et privés : configurez un [service VPN IPSec strongSwan![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://www.strongswan.org/about.html) directement dans votre cluster. Le service VPN IPSec strongSwan fournit un canal de communication de bout en bout sécurisé sur Internet, basé sur l'ensemble de protocoles IPSec (Internet Protocol Security) aux normes de l'industrie. Pour configurer une connexion sécurisée entre votre cluster et un réseau sur site, [configurez et déployez le service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) directement dans un pod de votre cluster.
* Noeuds worker connectés à un VLAN privé uniquement : configurez un noeud final VPN IPSec sur un périphérique de passerelle, par exemple, un dispositif Vyatta (Virtual Router Appliance). Ensuite, [configurez le service VPN IPSec strongSwan](/docs/containers?topic=containers-vpn#vpn-setup) dans votre cluster pour l'utilisation du noeud final VPN sur votre passerelle. Si vous ne souhaitez pas utiliser strongSwan, vous pouvez [configurer une connectivité VPN directement avec VRA](/docs/containers?topic=containers-vpn#vyatta).

</br>

### Communication externe avec des applications qui s'exécutent sur des noeuds worker
{: #external-workers}

Autorisez les demandes de trafic publiques ou privées depuis l'extérieur du cluster vers vos applications qui s'exécutent sur des noeuds worker.
{: shortdesc}

**Trafic privé vers des applications de cluster**</br>
Lorsque vous déployez une application dans votre cluster, vous souhaitez peut-être rendre l'application accessible uniquement aux utilisateurs et services qui se trouvent sur le même réseau privé que votre cluster. L'équilibrage de charge privé est idéal pour rendre votre application disponible pour des demandes depuis l'extérieur du cluster sans exposer l'application au public général. Vous pouvez également utiliser l'équilibrage de charge privé pour tester des accès, demander du routage et d'autres configurations pour votre application avant de l'exposer ultérieurement au public avec des services réseau public. Pour autoriser les demandes de trafic privées depuis l'extérieur du cluster vers vos applications, vous pouvez créer des services de mise en réseau Kubernetes privés, tels que des ports de noeud privés, des NLB et des ALB Ingress. Vous pouvez ensuite utiliser des règles pre-DNAT Calico pour bloquer le trafic vers des ports de noeud publics de services de mise en réseau privés. Pour plus d'informations, voir [Planification de l'équilibrage de charge externe privé](/docs/containers?topic=containers-cs_network_planning#private_access).

**Trafic public vers des applications de cluster**</br>
Pour rendre vos applications accessibles de façon externe à partir de l'internet public, vous pouvez créer des ports de noeud publics, des équilibreurs de charge de réseau (NLB) et des équilibreurs de charge d'application (ALB) Ingress. Les services de mise en réseau publics se connectent à cette interface de réseau public en fournissant à votre application une adresse IP publique et, le cas échéant, une URL publique. Lorsqu'une application est exposée au public, quiconque disposant de l'adresse IP de service public ou de l'URL que vous avez configurée pour votre application peut envoyer une demande à cette dernière. Vous pouvez ensuite utiliser des règles pre-DNAT Calico pour contrôler le trafic vers les services de mise en réseau publics, par exemple, en plaçant sur liste blanche le trafic provenant de certaines adresses IP source ou certains routages CIDR et en bloquant tous les autres trafics. Pour plus d'informations, voir [Planification de l'équilibrage de charge externe public](/docs/containers?topic=containers-cs_network_planning#private_access).

Pour plus de sécurité, isolez les charges de travail sur le réseau avec des noeuds worker de périphérie. Les noeuds worker de périphérie peuvent améliorer la sécurité de votre cluster en limitant les accès depuis l'extérieur aux noeuds worker qui sont connectés à des VLAN publics et en isolant la charge de travail du réseau. Lorsque vous [labellisez des noeuds worker en tant que noeuds de périphérie](/docs/containers?topic=containers-edge#edge_nodes), les pods NLB et ALB se déploient uniquement sur les noeuds worker spécifiés. Pour éviter l'exécution d'autres charges de travail sur les noeuds de périphérie, vous pouvez [ajouter une annotation taint aux noeuds de périphérie](/docs/containers?topic=containers-edge#edge_workloads).
Dans Kubernetes versions 1.14 et ultérieures, vous pouvez déployer des NLB et des ALB publics et privés sur des noeuds de périphérie.
Par exemple, si vos noeuds worker sont connectés à un VLAN privé uniquement et que vous devez autoriser l'accès public à une application de votre cluster, vous pouvez créer un pool de noeuds worker de périphérie dans lequel les noeuds de périphérie sont connectés à des VLAN publics et privés. Vous pouvez déployer des NLB et des ALB publics sur ces noeuds de périphérie afin de garantir que seuls ces noeuds worker peuvent gérer les connexions publiques. 

Si vos noeuds worker sont connectés à un VLAN privé uniquement et que vous utilisez un périphérique de passerelle pour fournir la communication entre les noeuds worker et le maître cluster, vous pouvez également configurer le périphérique comme pare-feu public ou privé. Pour autoriser les demandes de trafic privées depuis l'extérieur du cluster vers vos applications, vous pouvez créer des ports de noeud privés ou publics, des NLB et des ALB Ingress. Ensuite, vous devez [ouvrir les ports et les adresses IP requis](/docs/containers?topic=containers-firewall#firewall_inbound) dans votre pare-feu de périphérique de passerelle pour autoriser le trafic entrant vers ces services sur le réseau public ou privé.
{: note}

<br />


## Scénario : Exécution de charges de travail d'application accessible sur Internet dans un cluster
{: #internet-facing}

Dans ce scénario, vous souhaitez exécuter des charges de travail dans un cluster qui sont accessibles aux demandes émises depuis l'Internet de sorte que les utilisateurs finals puissent accéder à vos applications. Vous souhaitez utiliser l'option permettant d'isoler l'accès public dans votre cluster et l'option permettant de contrôler les demandes publiques autorisées vers votre cluster. En outre, vos noeuds worker accèdent automatiquement aux services {{site.data.keyword.Bluemix_notm}} que vous souhaitez connecter à votre cluster.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_internet.png" alt="Image de l'architecture d'un cluster qui exécute des charges de travail accessibles sur Internet"/>
 <figcaption>Architecture d'un cluster qui exécute des charges de travail accessibles sur Internet</figcaption>
</figure>
</p>

Pour réaliser cette configuration, vous créez un cluster en connectant des noeuds worker à des VLAN publics et privés. 

Si vous créez le cluster avec des VLAN publics et privés, vous ne pourrez plus retirer tous les VLAN publics de ce cluster par la suite. Le retrait de tous les VLAN publics d'un cluster peut provoquer l'arrêt de plusieurs composants du cluster. A la place, créez un nouveau pool de noeuds worker qui est connecté à un VLAN privé uniquement.
{: note}

Vous pouvez choisir d'autoriser la communication entre les noeuds worker et le maître et entre les utilisateurs et le maître sur les réseaux public et privé ou sur le réseau public uniquement. 
* Noeuds finaux de service public et privé : votre compte doit être activé avec la fonction VRF et activé pour utiliser des noeuds finaux de service. La communication entre les noeuds worker et le maître est établie sur le réseau privé via le noeud final de service privé et sur le réseau public via le noeud final de service public. Le maître est accessible au public pour les utilisateurs de cluster autorisés via le noeud final de service public. 
* Noeud final de service public : Si vous ne souhaitez ou ne pouvez pas activer VRF pour votre compte, vos noeuds worker et les utilisateurs de cluster autorisés peuvent automatiquement se connecter au maître Kubernetes sur le réseau public via le noeud final de service public. 

Vos noeuds worker peuvent communiquer automatiquement et en toute sécurité avec d'autres services {{site.data.keyword.Bluemix_notm}} qui prennent en charge des noeuds finaux de service privé sur votre réseau privé d'infrastructure IBM Cloud (SoftLayer). Si un service {{site.data.keyword.Bluemix_notm}} ne prend pas en charge des noeuds finaux de service privé, les noeuds worker peuvent communiquer en toute sécurité avec les services sur le réseau public. Vous pouvez verrouiller les interfaces publiques ou privées de noeuds worker en utilisant les règles réseau Calico pour l'isolement de réseau privé ou de réseau public. Vous devrez peut-être autoriser l'accès aux adresses IP publiques et privées des services que vous souhaitez utiliser dans ces règles d'isolement Calico. 

Pour exposer une application dans votre cluster sur Internet, vous pouvez créer un équilibreur de charge de réseau (NLB) public ou un équilibreur de charge d'application (ALB) Ingress. Vous pouvez améliorer la sécurité de votre cluster en créant un pool de noeuds worker qui sont labellisés noeuds de périphérie. Les pods des services de réseau public sont déployés sur des noeuds de périphérie de sorte que les charges de travail de trafic externe soient isolées pour un petit nombre de noeuds worker dans votre cluster. Vous pouvez contrôler davantage le trafic public vers les services de réseau qui exposent vos applications en créant des règles pre-DNAT Calico, telles que des règles d'inscription sur liste blanche et des règles d'inscription sur liste noire. 

Si vos noeuds worker doivent accéder à des services dans des réseaux privés en dehors de votre compte {{site.data.keyword.Bluemix_notm}}, vous pouvez configurer et déployer le service VPN IPSec strongSwan dans votre cluster ou optimiser les services {{site.data.keyword.Bluemix_notm}} Direct Link pour vous connecter à ces services. 

Prêt à utiliser un cluster pour ce scénario ? Après avoir planifié vos configurations de [haute disponibilité](/docs/containers?topic=containers-ha_clusters) et de [noeud worker](/docs/containers?topic=containers-planning_worker_nodes), reportez-vous à la rubrique [Création de clusters](/docs/containers?topic=containers-clusters#cluster_prepare).

<br />


## Scénario : Extension de votre centre de données sur site à un cluster sur le réseau privé et ajout d'un accès public limité
{: #limited-public}

Dans ce scénario, vous souhaitez exécuter des charges de travail dans un cluster qui sont accessibles aux services, bases de données ou autres ressources de votre centre de données sur site. Cependant, vous pouvez être amené à fournir un accès public limité à votre cluster, et vous souhaitez vous assurer que les accès publics sont contrôlés et isolés dans votre cluster. Par exemple, vous pouvez avoir besoin que vos noeuds worker accèdent à un service {{site.data.keyword.Bluemix_notm}} qui ne prend pas en charge les noeuds finaux de service privé et que ce service soit accessible sur le réseau public. Ou, vous pouvez être amené à fournir un accès public limité à une application qui s'exécute dans votre cluster.
{: shortdesc}

Pour réaliser cette configuration de cluster, vous pouvez créer un pare-feu en [utilisant des noeuds de périphérie et des règles réseau Calico](#calico-pc) ou [en utilisant un périphérique de passerelle](#vyatta-gateway).

### Utilisation de noeuds de périphérie et de règles réseau Calico
{: #calico-pc}

Autorisez une connectivité publique limitée à votre cluster en utilisant des noeuds de périphérie en tant que passerelle publique et des règles réseau Calico en tant que pare-feu public.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_calico.png" alt="Image de l'architecture d'un cluster qui utilise des noeuds de périphérie et des règles réseau Calico pour un accès public sécurisé"/> <figcaption>Architecture d'un cluster qui utilise des noeuds de périphérie et des règles réseau Calico pour un accès public sécurisé</figcaption>
</figure>
</p>

Avec cette configuration, vous créez un cluster en connectant des noeuds worker à un VLAN privé uniquement. Votre compte doit être activé avec la fonction VRF et activé pour l'utilisation de noeuds finaux de service privé. 

Le maître Kubernetes est accessible via le noeud final de service privé si les utilisateurs de cluster autorisés se trouvent dans votre réseau privé {{site.data.keyword.Bluemix_notm}} ou s'ils sont connectés au réseau privé via une [connexion VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) ou [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Toutefois, la communication avec le maître Kubernetes via le noeud final de service privé doit passer par la plage d'adresses IP <code>166.X.X.X</code>, qui n'est pas routable à partir d'une connexion VPN, ou par {{site.data.keyword.Bluemix_notm}} Direct Link. Vous pouvez exposer le noeud final de service privé du maître pour vos utilisateurs de cluster en utilisant un équilibreur de charge de réseau (NLB) privé. Le NLB privé expose le noeud final de service privé du maître en tant que plage d'adresses IP de cluster <code>10.X.X.X</code> interne auxquelles les utilisateurs peuvent accéder à l'aide de la connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link. Si vous activez uniquement le noeud final de service privé, vous pouvez utiliser le tableau de bord Kubernetes ou activer temporairement le noeud final de service public pour créer le NLB privé.


Ensuite, vous pouvez créer un pool de noeuds worker qui sont connectés aux VLAN public et privé et labellisés noeuds de périphérie. Les noeuds worker de périphérie peuvent améliorer la sécurité de votre cluster en limitant les accès en externe à quelques noeuds worker seulement et en isolant la charge de travail du réseau dans ces noeuds worker.


Vos noeuds worker peuvent communiquer automatiquement et en toute sécurité avec d'autres services {{site.data.keyword.Bluemix_notm}} qui prennent en charge des noeuds finaux de service privé sur votre réseau privé d'infrastructure IBM Cloud (SoftLayer). Si un service {{site.data.keyword.Bluemix_notm}} ne prend pas en charge des noeuds finaux de service privé, vos noeuds de périphérie qui sont connectés à un VLAN public peuvent communiquer en toute sécurité avec les services sur le réseau public. Vous pouvez verrouiller les interfaces publiques ou privées de noeuds worker en utilisant les règles réseau Calico pour l'isolement de réseau privé ou de réseau public. Vous devrez peut-être autoriser l'accès aux adresses IP publiques et privées des services que vous souhaitez utiliser dans ces règles d'isolement Calico. 

Pour fournir un accès privé à une application de votre cluster, vous pouvez créer un équilibreur de charge de réseau (NLB) privé ou un équilibreur de charge d'application (ALB) Ingress afin d'exposer votre application sur le réseau privé uniquement. Vous pouvez bloquer l'ensemble du trafic public vers les services de réseau qui exposent vos applications en créant des règles pre-DNAT Calico, telles que des règles visant à bloquer des ports de noeud publics sur les noeuds worker. Si vous devez fournir un accès public limité à une application de votre cluster, vous pouvez créer un NLB ou ALB public afin d'exposer votre application. Vous devez ensuite déployer vos applications sur ces noeuds de périphérie de sorte que les NLB ou ALB puissent diriger le trafic public vers vos pods d'application. Vous pouvez contrôler davantage le trafic public vers les services de réseau qui exposent vos applications en créant des règles pre-DNAT Calico, telles que des règles d'inscription sur liste blanche et des règles d'inscription sur liste noire. Les pods des services de réseau public et privé sont déployés sur des noeuds de périphérie de sorte que les charges de travail de trafic externe soient isolées pour un petit nombre de noeuds worker dans votre cluster.   

Pour accéder en toute sécurité à des services en dehors de {{site.data.keyword.Bluemix_notm}} et d'autres réseaux sur site, vous pouvez configurer et déployer le service VPN IPSec strongSwan dans votre cluster. Le pod d'équilibreur de charge strongSwan est déployé sur un noeud worker du pool de noeuds worker de périphérie, où le pod établit une connexion sécurisée avec le réseau sur site via un tunnel VPN chiffré sur le réseau public. Vous pouvez aussi utiliser des services {{site.data.keyword.Bluemix_notm}} Direct Link pour connecter votre cluster à votre centre de données sur site sur le réseau privé uniquement.

Prêt à utiliser un cluster pour ce scénario ? Après avoir planifié vos configurations de [haute disponibilité](/docs/containers?topic=containers-ha_clusters) et de [noeud worker](/docs/containers?topic=containers-planning_worker_nodes), reportez-vous à la rubrique [Création de clusters](/docs/containers?topic=containers-clusters#cluster_prepare).

</br>

### Utilisation d'un périphérique de passerelle
{: #vyatta-gateway}

Autorisez une connectivité publique limitée à votre cluster en configurant un périphérique de passerelle, tel qu'un dispositif Vyatta (Virtual Router Appliance), en tant que passerelle publique et pare-feu public.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_gateway.png" alt="Image de l'architecture d'un cluster qui utilise un périphérique de passerelle pour un accès public sécurisé"/>
 <figcaption>Architecture d'un cluster qui utilise un périphérique de passerelle pour un accès public sécurisé</figcaption>
</figure>
</p>

Si vous configurez vos noeuds worker sur un VLAN privé uniquement et si vous ne souhaitez ou ne pouvez pas activer la fonction VRF pour votre compte, vous devez configurer un périphérique de passerelle pour fournir la connectivité de réseau entre vos noeuds worker et le maître sur le réseau public. Par exemple, vous pouvez choisir de configurer un [dispositif de routeur virtuel (VRA)](/docs/infrastructure/virtual-router-appliance?topic=virtual-router-appliance-about-the-vra) ou un [dispositif de sécurité Fortigate (FSA)](/docs/services/vmwaresolutions/services?topic=vmware-solutions-fsa_considerations).

Vous pouvez configurer votre périphérique de passerelle avec des règles réseau personnalisées afin d'assurer une sécurité réseau dédiée pour votre cluster et détecter et parer à des intrusions réseau. Lorsque vous configurez un pare-feu sur le réseau public, vous devez ouvrir les ports et les adresses IP privées requis pour chaque région de manière à permettre au maître et aux noeuds worker de communiquer.
Si vous configurez également ce pare-feu sur le réseau privé, vous devez également ouvrir les ports et les adresses IP privées requis de manière à permettre la communication entre les noeuds worker et permettre à votre cluster d'accéder aux ressources d'infrastructure sur le réseau privé.
Vous devez également activer la fonction Spanning VLAN pour votre compte de sorte que les sous-réseaux puissent effectuer le routage sur le même VLAN et sur les VLAN.

Pour connecter vos noeuds worker et vos applications de manière sécurisée à un réseau sur site ou à des services en dehors de {{site.data.keyword.Bluemix_notm}}, configurez un noeud final VPN IPSec sur votre périphérique de passerelle et le service VPN IPSec strongSwan dans votre cluster pour utiliser le noeud final VPN de passerelle. Si vous ne souhaitez pas utiliser strongSwan, vous pouvez configurer une connectivité VPN directement avec VRA. 

Vos noeuds worker peuvent communiquer en toute sécurité avec d'autres services {{site.data.keyword.Bluemix_notm}} et services publics en dehors de {{site.data.keyword.Bluemix_notm}} via votre périphérique de passerelle. Vous pouvez configurer votre pare-feu pour autoriser l'accès aux adresses IP publiques et privées des seuls services que vous souhaitez utiliser. 

Pour fournir un accès privé à une application de votre cluster, vous pouvez créer un équilibreur de charge de réseau (NLB) privé ou un équilibreur de charge d'application (ALB) Ingress afin d'exposer votre application sur le réseau privé uniquement. Si vous devez fournir un accès public limité à une application de votre cluster, vous pouvez créer un NLB ou ALB public afin d'exposer votre application. Etant donné que l'ensemble du trafic passe par votre pare-feu de périphérique de passerelle, vous pouvez contrôler le trafic public vers les services de réseau qui exposent vos applications en ouvrant les ports et les adresses IP des services dans votre pare-feu pour permettre le trafic entrant vers ces services. 

Prêt à utiliser un cluster pour ce scénario ? Après avoir planifié vos configurations de [haute disponibilité](/docs/containers?topic=containers-ha_clusters) et de [noeud worker](/docs/containers?topic=containers-planning_worker_nodes), reportez-vous à la rubrique [Création de clusters](/docs/containers?topic=containers-clusters#cluster_prepare).

<br />


## Scénario : Extension de votre centre de données sur site à un cluster sur le réseau privé
{: #private_clusters}

Dans ce scénario, vous souhaitez exécuter des charges de travail dans un cluster {{site.data.keyword.containerlong_notm}}. Toutefois, vous souhaitez que ces charges de travail soient accessibles uniquement aux services, bases de données ou autres ressources de votre centre de données sur site, par exemple, {{site.data.keyword.icpfull_notm}}. Vos charges de travail de cluster auront peut-être besoin d'accéder à un petit nombre d'autres services {{site.data.keyword.Bluemix_notm}} qui prennent en charge la communication sur le réseau privé, par exemple, {{site.data.keyword.cos_full_notm}}.
{: shortdesc}

<p>
<figure>
 <img src="images/cs_clusters_planning_extend.png" alt="Image de l'architecture d'un cluster qui se connecte à un centre de données sur site sur le réseau privé"/>
 <figcaption>Architecture d'un cluster qui se connecte à un centre de données sur site sur le réseau privé</figcaption>
</figure>
</p>

Pour réaliser cette configuration, vous créez un cluster en connectant des noeuds worker à un VLAN privé uniquement. Pour fournir la connectivité entre le maître cluster et les noeuds worker sur le réseau privé via le noeud final de service privé uniquement, votre compte doit être activé avec la fonction VRF et activé pour l'utilisation de noeuds finaux de service. Etant donné que votre cluster est visible pour n'importe quelle ressource sur le réseau privé lorsque la fonction VRF est activée, vous pouvez isoler votre cluster des autres systèmes sur le réseau privé en appliquant des règles réseau privé Calico. 

Le maître Kubernetes est accessible via le noeud final de service privé si les utilisateurs de cluster autorisés se trouvent dans votre réseau privé {{site.data.keyword.Bluemix_notm}} ou s'ils sont connectés au réseau privé via une [connexion VPN](/docs/infrastructure/iaas-vpn?topic=VPN-gettingstarted-with-virtual-private-networking) ou [{{site.data.keyword.Bluemix_notm}} Direct Link](/docs/infrastructure/direct-link?topic=direct-link-get-started-with-ibm-cloud-direct-link). Toutefois, la communication avec le maître Kubernetes via le noeud final de service privé doit passer par la plage d'adresses IP <code>166.X.X.X</code>, qui n'est pas routable à partir d'une connexion VPN, ou par {{site.data.keyword.Bluemix_notm}} Direct Link. Vous pouvez exposer le noeud final de service privé du maître pour vos utilisateurs de cluster en utilisant un équilibreur de charge de réseau (NLB) privé. Le NLB privé expose le noeud final de service privé du maître en tant que plage d'adresses IP de cluster <code>10.X.X.X</code> interne auxquelles les utilisateurs peuvent accéder à l'aide de la connexion VPN ou {{site.data.keyword.Bluemix_notm}} Direct Link. Si vous activez uniquement le noeud final de service privé, vous pouvez utiliser le tableau de bord Kubernetes ou activer temporairement le noeud final de service public pour créer le NLB privé.


Vos noeuds worker peuvent communiquer automatiquement et en toute sécurité avec d'autres services {{site.data.keyword.Bluemix_notm}} qui prennent en charge des noeuds finaux de service privé, par exemple, {{site.data.keyword.registrylong}}, sur votre réseau privé d'infrastructure IBM Cloud (SoftLayer). Par exemple, des environnements de matériel dédié pour toutes les instances de plan standard de {{site.data.keyword.cloudant_short_notm}} prennent en charge des noeuds finaux de service privé. Si un service {{site.data.keyword.Bluemix_notm}} ne prend pas en charge des noeuds finaux de service privé, votre cluster ne peut pas accéder à ce service.

Pour fournir un accès privé à une application de votre cluster, vous pouvez créer un équilibreur de charge de réseau (NLB) privé ou un équilibreur de charge d'application (ALB) Ingress. Ces services de réseau Kubernetes exposent votre application sur le réseau privé uniquement de sorte que n'importe quel système sur site ayant une connexion au sous-réseau sur lequel se trouve l'adresse IP NLB puisse accéder à l'application. 

Prêt à utiliser un cluster pour ce scénario ? Après avoir planifié vos configurations de [haute disponibilité](/docs/containers?topic=containers-ha_clusters) et de [noeud worker](/docs/containers?topic=containers-planning_worker_nodes), reportez-vous à la rubrique [Création de clusters](/docs/containers?topic=containers-clusters#cluster_prepare).
