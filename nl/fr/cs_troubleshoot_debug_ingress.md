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


# Débogage d'Ingress
{: #cs_troubleshoot_debug_ingress}

Lorsque vous utilisez {{site.data.keyword.containerlong}}, tenez compte de ces techniques pour identifier et résoudre les incidents liés à Ingress et procéder au débogage.
{: shortdesc}

Vous avez exposé votre application au public en créant une ressource Ingress pour votre application dans votre cluster. Cependant, lorsque vous tentez de vous connecter à votre application via le sous-domaine ou l'adresse IP publique de l'ALB, la connexion échoue ou arrive à expiration. Les étapes indiquées dans les sections suivantes peuvent vous aider à déboguer votre configuration d'Ingress.

Veillez à définir un hôte dans une seule ressource Ingress. Si un hôte est défini dans plusieurs ressources Ingress, l'ALB risque de ne pas acheminer le trafic correctement et vous pourrez obtenir des erreurs.
{: tip}

Avant de commencer, vérifiez que vous disposez des [règles d'accès {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) suivantes :
  - Rôle de plateforme **Editeur** ou **Administrateur** pour le cluster
  - Rôle de service **Auteur** ou **Responsable**

## Etape 1 : Exécution de tests Ingress dans l'outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}}

Lorsque vous traitez les incidents, vous pouvez utiliser l'outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}} pour exécuter des tests Ingress et regrouper des informations pertinentes sur Ingress. Pour utiliser l'outil de débogage, installez la [charte Helm `ibmcloud-iks-debug` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/kubernetes/helm/iks-charts/ibmcloud-iks-debug) :
{: shortdesc}


1. [Configurez Helm dans votre cluster, créez un compte de service pour Tiller et ajoutez le référentiel `ibm` dans votre instance Helm](/docs/containers?topic=containers-helm).

2. Installez la charte Helm dans votre cluster.
  ```
  helm install iks-charts/ibmcloud-iks-debug --name debug-tool
  ```
  {: pre}


3. Démarrez un serveur proxy pour afficher l'interface de l'outil de débogage.
  ```
  kubectl proxy --port 8080
  ```
  {: pre}

4. Dans un navigateur Web, ouvrez l'URL de l'interface de l'outil de débogage : http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. Sélectionnez la série de tests **ingress**. Certains tests vérifient les avertissements, erreurs ou problèmes potentiels, d'autres se contentent de rassembler des informations que vous pouvez utiliser comme référence lorsque vous traitez les incidents. Pour plus d'informations sur la fonction de chaque test, cliquez sur l'icône d'information en regard du nom du test.

6. Cliquez sur **Exécuter**.

7. Vérifiez les résultats de chaque test.
  * Si un test échoue, cliquez sur l'icône d'information à côté du nom du test dans la colonne de gauche pour plus d'informations sur la manière de résoudre le problème.
  * Vous pouvez également utiliser les résultats des tests uniquement pour recueillir des informations lors du débogage de votre service Ingress dans les sections suivantes.

## Etape 2 : Recherche de messages d'erreur dans le déploiement de votre ressource Ingress et dans les journaux de pod de l'équilibreur de charge d'application (ALB)
{: #errors}

Commencez par vérifier les messages d'erreur dans les événements de déploiement de la ressource Ingress et les journaux de pod d'ALB. Ces messages d'erreur peuvent vous aider à trouver la cause première des erreurs, puis à déboguer votre configuration Ingress dans les sections suivantes.
{: shortdesc}

1. Vérifiez le déploiement de votre ressource Ingress et recherchez les messages d'avertissement et d'erreur.
    ```
    kubectl describe ingress <myingress>
    ```
    {: pre}

    Dans la section **Events** de la sortie, vous pourrez voir des messages d'avertissement signalant des valeurs non valides dans votre ressource Ingress ou dans certaines annotations que vous avez utilisées. Consultez la [documentation sur la configuration de la ressource Ingress](/docs/containers?topic=containers-ingress#public_inside_4) ou la [documentation sur les annotations](/docs/containers?topic=containers-ingress_annotation).

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
2. Vérifiez le statut de vos pods d'ALB.
    1. Obtenez les pods d'ALB qui s'exécutent dans votre cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    2. Vérifiez que tous les pods sont opérationnels en examinant la colonne **STATUS**.

    3. Si un pod n'est pas en cours d'exécution (`Running`), vous pouvez désactiver puis réactiver l'ALB. Dans les commandes suivantes, remplacez `<ALB_ID>` par l'ID de l'ALB du pod. Par exemple, si le pod qui n'est pas en cours d'exécution est nommé `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1-5d6d86fbbc-kxj6z`, l'ID de l'ALB est `public-crb2f60e9735254ac8b20b9c1e38b649a5-alb1`.
        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --disable
        ```
        {: pre}

        ```
        ibmcloud ks alb-configure --albID <ALB_ID> --enable
        ```
        {: pre}

3. Vérifiez les journaux de votre équilibreur de charge ALB.
    1.  Obtenez les ID des pods d'ALB en cours d'exécution dans votre cluster.
        ```
        kubectl get pods -n kube-system | grep alb
        ```
        {: pre}

    3. Obtenez les journaux correspondant au conteneur `nginx-ingress` sur chaque pod d'ALB.
        ```
        kubectl logs <ingress_pod_ID> nginx-ingress -n kube-system
        ```
        {: pre}

    4. Recherchez les messages d'erreur dans les journaux de l'équilibreur de charge ALB.

## Etape 3 : Exécution d'une commande ping sur le sous-domaine et les adresses IP publiques de l'ALB
{: #ping}

Vérifiez la disponibilité du sous-domaine Ingress et des adresses IP publiques des ALB.
{: shortdesc}

1. Obtenez les adresses IP sur lesquelles vos ALB publics sont à l'écoute.
    ```
    ibmcloud ks albs --cluster <cluster_name_or_ID>
    ```
    {: pre}

    Exemple de sortie d'un cluster à zones multiples avec des noeuds worker dans les zones `dal10` et `dal13` :

    ```
    ALB ID                                            Enabled   Status     Type      ALB IP          Zone    Build                          ALB VLAN ID
    private-cr24a9f2caf6554648836337d240064935-alb1   false     disabled   private   -               dal13   ingress:411/ingress-auth:315   2294021
    private-cr24a9f2caf6554648836337d240064935-alb2   false     disabled   private   -               dal10   ingress:411/ingress-auth:315   2234947
    public-cr24a9f2caf6554648836337d240064935-alb1    true      enabled    public    169.62.196.238  dal13   ingress:411/ingress-auth:315   2294019
    public-cr24a9f2caf6554648836337d240064935-alb2    true      enabled    public    169.46.52.222   dal10   ingress:411/ingress-auth:315   2234945
    ```
    {: screen}

    * Si un ALB public n'a pas d'adresse IP, voir [L'ALB Ingress ne se déploie pas dans une zone](/docs/containers?topic=containers-cs_troubleshoot_network#cs_subnet_limit).

2. Vérifiez l'intégrité des adresses IP de votre ALB.

    * Pour les clusters à zone unique ou à zones multiples : exécutez une commande ping sur l'adresse IP de chaque ALB public pour vérifier que chaque ALB est en mesure de recevoir des paquets. Si vous utilisez des ALB privés, vous pouvez exécuter une commande ping sur leurs adresses IP uniquement à partir du réseau privé.
        ```
        ping <ALB_IP>
        ```
        {: pre}

        * Si l'interface CLI renvoie un dépassement de délai et que vous disposez d'un pare-feu personnalisé pour protéger vos noeuds worker, vérifiez que vous avez autorisé ICMP dans votre [pare-feu](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall).
        * S'il n'y a aucun pare-feu bloquant les commandes ping et que ces commandes s'exécutent puis renvoient un dépassement de délai, [vérifiez le statut de vos pods d'ALB](#check_pods).

    * Clusters à zones multiples uniquement : vous pouvez utiliser le diagnostic d'intégrité de l'équilibreur de charge MZLB pour déterminer le statut des adresses IP de votre ALB. Pour plus d'informations sur l'équilibreur de charge MZLB, voir [Equilibreur de charge pour zones multiples (MZLB)](/docs/containers?topic=containers-ingress#planning). Le diagnostic d'intégrité de l'équilibreur de charge MZLB est disponible uniquement pour les clusters dont le nouveau sous-domaine Ingress est au format `<cluster_name>.<region_or_zone>.containers.appdomain.cloud`. Si votre cluster utilise encore l'ancien format `<cluster_name>.<region>.containers.mybluemix.net`, [convertissez votre cluster à une seule zone en un cluster à zones multiples](/docs/containers?topic=containers-add_workers#add_zone). Un sous-domaine au nouveau format est affecté à votre cluster, mais celui-ci peut continuer à utiliser l'ancien format de sous-domaine. Vous pouvez aussi commander un nouveau cluster auquel le nouveau format de sous-domaine sera automatiquement affecté.

    La commande curl HTTP suivante utilise l'hôte `albhealth`, qui est configuré par {{site.data.keyword.containerlong_notm}} de sorte à renvoyer le statut `healthy` ou `unhealthy` pour une adresse IP d'ALB.
        ```
        curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
        ```
        {: pre}

        Exemple de sortie :
        ```
        healthy
        ```
        {: screen}
        Si une ou plusieurs adresses IP renvoient `unhealthy`, [vérifiez le statut des pods d'ALB](#check_pods).

3. Obtenez le sous-domaine Ingress fourni par IBM.
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID> | grep Ingress
    ```
    {: pre}

    Exemple de sortie :
    ```
    Ingress Subdomain:      mycluster-12345.us-south.containers.appdomain.cloud
    Ingress Secret:         <tls_secret>
    ```
    {: screen}

4. Vérifiez que les adresses IP de chaque ALB public que vous avez obtenues à l'étape 2 de cette section sont enregistrées avec le sous-domaine Ingress fourni par IBM de votre cluster. Par exemple, dans un cluster à zones multiples, l'adresse IP de l'ALB public dans chaque zone contenant vos noeuds worker doit être enregistrée sous le même nom d'hôte.

    ```
    kubectl get ingress -o wide
    ```
    {: pre}

    Exemple de sortie :
    ```
    NAME                HOSTS                                                    ADDRESS                        PORTS     AGE
    myingressresource   mycluster-12345.us-south.containers.appdomain.cloud      169.46.52.222,169.62.196.238   80        1h
    ```
    {: screen}

## Etape 4 : Vérification de vos mappages de domaine et de la configuration de la ressource Ingress
{: #ts_ingress_config}

1. Si vous utilisez un domaine personnalisé, vérifiez que vous avez utilisé votre fournisseur de DNS pour mapper le domaine personnalisé au sous-domaine fourni par IBM ou à l'adresse IP publique de l'ALB. Notez que l'utilisation de CNAME est privilégiée car IBM fournit des diagnostics d'intégrité automatiques sur le sous-domaine IBM et retire toutes les adresses IP défaillantes dans la réponse DNS.
    * Sous-domaine fourni par IBM : vérifiez que votre domaine personnalisé est mappé au sous domaine fourni par IBM du cluster dans l'enregistrement CNAME (Canonical Name).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Exemple de sortie :
        ```
        www.my-domain.com is an alias for mycluster-12345.us-south.containers.appdomain.cloud
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
        mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
        ```
        {: screen}

    * Adresse IP publique : vérifiez que votre domaine personnalisé est mappé à l'adresse IP publique portable de l'ALB dans l'enregistrement A. Les adresses IP doivent correspondre aux adresses IP d'ALB publiques que vous avez obtenues à l'étape 1 de la [section précédente](#ping).
        ```
        host www.my-domain.com
        ```
        {: pre}

        Exemple de sortie :
        ```
        www.my-domain.com has address 169.46.52.222
        www.my-domain.com has address 169.62.196.238
        ```
        {: screen}

2. Vérifiez les fichiers de configuration de la ressource Ingress pour votre cluster.
    ```
    kubectl get ingress -o yaml
    ```
    {: pre}

    1. Veillez à définir un hôte dans une seule ressource Ingress. Si un hôte est défini dans plusieurs ressources Ingress, l'ALB risque de ne pas acheminer le trafic correctement et vous pourrez obtenir des erreurs.

    2. Vérifiez que le sous-domaine et le certificat TLS sont corrects. Pour obtenir le certificat TLS et le sous-domaine Ingress fournis par IBM, exécutez la commande  `ibmcloud ks cluster-get --cluster <cluster_name_or_ID>`.

    3.  Assurez-vous que votre application est en mode écoute sur le même chemin que celui qui est configuré dans la section **path** de votre ressource Ingress. Si votre application est configurée pour être en mode écoute à la racine, utilisez `/` comme chemin. Si le trafic entrant dans ce chemin doit être acheminé vers un autre chemin que celui où votre application est à l'écoute, utilisez l'annotation des [chemins de redirection (rewrite path)](/docs/containers?topic=containers-ingress_annotation#rewrite-path).

    4. Editez le fichier YAML de configuration de votre ressource selon les besoins. Lorsque vous fermez l'éditeur, vos modifications sont sauvegardées et automatiquement appliquées.
        ```
        kubectl edit ingress <myingressresource>
        ```
        {: pre}

## Retrait d'un équilibreur de charge d'application (ALB) du DNS à des fins de débogage
{: #one_alb}

Si vous ne parvenez pas à accéder à votre application via une adresse IP d'ABL spécifique, vous pouvez retirer provisoirement l'ALB de l'environnement de production en désactivant son enregistrement DNS. Vous pouvez ensuite utiliser l'adresse IP de l'ALB pour exécuter des tests de débogage sur cet ALB.

Par exemple, admettons que vous disposez d'un cluster à zones multiples dans 2 zones et que 2 ALB publics aient les adresses IP `169.46.52.222` et `169.62.196.238`. Bien que le diagnostic d'intégrité indique que l'ALB de la deuxième zone est sain (healthy), votre application n'est pas accessible directement en l'utilisant. Vous décidez de retirer l'adresse IP de cet ALB, `169.62.196.238`, de l'environnement de production à des fins de débogage. L'adresse IP de la première zone, `169.46.52.222`, est enregistrée avec votre domaine et continue à router le trafic pendant que vous déboguez l'ALB de la deuxième zone.

1. Obtenez le nom de l'ALB dont l'adresse IP est inaccessible.
    ```
    ibmcloud ks albs --cluster <cluster_name> | grep <ALB_IP>
    ```
    {: pre}

    Par exemple, l'adresse IP `169.62.196.238` qui est inaccessible appartient à l'ALB `public-cr24a9f2caf6554648836337d240064935-alb1` :
    ```
    ALB ID                                            Enabled   Status     Type      ALB IP           Zone    Build                          ALB VLAN ID
    public-cr24a9f2caf6554648836337d240064935-alb1    false     disabled   private   169.62.196.238   dal13   ingress:411/ingress-auth:315   2294021
    ```
    {: screen}

2. En utilisant le nom de l'ALB indiqué à l'étape précédente, obtenez les noms des pods de l'ALB. La commande suivante utilise le nom de l'ALB indiqué comme exemple à l'étape précédente :
    ```
    kubectl get pods -n kube-system | grep public-cr24a9f2caf6554648836337d240064935-alb1
    ```
    {: pre}

    Exemple de sortie :
    ```
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq   2/2       Running   0          24m
    public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-trqxc   2/2       Running   0          24m
    ```
    {: screen}

3. Désactivez le diagnostic d'intégrité qui s'exécute sur tous les pods d'ALB. Répétez ces étapes pour chaque pod d'ALB que vous avez obtenu à l'étape précédente. Les exemples de commandes et de sorties indiqués dans ces étapes utilisent le premier pod, `public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq`.
    1. Connectez-vous au pod d'ALB et vérifiez la ligne du nom de serveur (`server_name`) dans le fichier de configuration NGINX.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Exemple de sortie confirmant que le pod d'ALB est configuré avec le nom d'hôte correct du diagnostic d'intégrité, `albhealth.<domain>` :
        ```
        server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud;
        ```
        {: screen}

    2. Pour retirer l'adresse IP en désactivant le diagnostic d'intégrité, insérez `#` au début de `server_name`. Lorsque l'hôte virtuel `albhealth.mycluster-12345.us-south.containers.appdomain.cloud` est désactivé pour l'ALB, le diagnostic d'intégrité automatisé retire automatiquement l'adresse IP de la réponse DNS.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- sed -i -e 's*server_name*#server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

    3. Vérifiez que la modification a été appliquée.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- grep server_name /etc/nginx/conf.d/kube-system-alb-health.conf
        ```
        {: pre}

        Exemple de sortie :
        ```
        #server_name albhealth.mycluster-12345.us-south.containers.appdomain.cloud
        ```
        {: screen}

    4. Pour retirer l'adresse IP de l'enregistrement DNS, rechargez la configuration NGINX.
        ```
        kubectl exec -ti public-cr24a9f2caf6554648836337d240064935-alb1-7f78686c9d-8rvtq -n kube-system -c nginx-ingress -- nginx -s reload
        ```
        {: pre}

    5. Répétez ces étapes pour chaque pod d'ALB.

4. Désormais, lorsque vous tentez d'exécuter la commande curl sur l'hôte `albhealth` pour effectuer un diagnostic d'intégrité sur l'adresse IP de l'ALB, le résultat est un échec.
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

    Sortie :
    ```
    <html>
        <head>
            <title>404 Not Found</title>
        </head>
        <body bgcolor="white"><center>
            <h1>404 Not Found</h1>
        </body>
    </html>
    ```
    {: screen}

5. Vérifiez que l'adresse IP de l'ALB est retirée de l'enregistrement DNS pour votre domaine en contrôlant le serveur Cloudflare. Notez que la mise à jour de l'enregistrement DNS peut prendre quelques minutes.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Exemple de sortie confirmant que seule l'adresse IP d'ALB saine, `169.46.52.222`, reste dans l'enregistrement DNS et que l'adresse IP d'ALB qui n'est pas saine, `169.62.196.238`, a été retirée :
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    ```
    {: screen}

6. Maintenant que l'adresse IP de l'ALB a été retirée de l'environnement de production, vous pouvez effectuer des tests de débogage sur votre application en l'utilisant. Pour tester la communication avec votre application via cette adresse IP, vous pouvez exécuter la commande cURL suivante, en remplaçant les valeurs indiquées en exemple par vos propres valeurs :
    ```
    curl -X GET --resolve mycluster-12345.us-south.containers.appdomain.cloud:443:169.62.196.238 https://mycluster-12345.us-south.containers.appdomain.cloud/
    ```
    {: pre}

    * Si tout est configuré correctement, vous obtenez la réponse prévue de votre application.
    * Si vous obtenez une erreur en réponse, une erreur a pu se produire dans votre application ou dans une configuration qui ne s'applique qu'à cet ALB spécifique. Vérifiez le code de votre application, les [fichiers de configuration de votre ressource Ingress](/docs/containers?topic=containers-ingress#public_inside_4) ou toute autre configuration ayant été appliquée uniquement à cet ALB.

7. Après avoir terminé le débogage, rétablissez le diagnostic d'intégrité sur tous les pods d'ALB. Répétez ces étapes pour chaque pod d'ALB.
  1. Connectez-vous au pod d'ALB et supprimez le signe `#` de `server_name`.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- sed -i -e 's*#server_name*server_name*g' /etc/nginx/conf.d/kube-system-alb-health.conf
    ```
    {: pre}

  2. Rechargez la configuration NGINX pour que le rétablissement du diagnostic d'intégrité soit appliqué.
    ```
    kubectl exec -ti <pod_name> -n kube-system -c nginx-ingress -- nginx -s reload
    ```
    {: pre}

9. Désormais, lorsque vous exécutez la commande cURL sur l'hôte `albhealth` pour effectuer un diagnostic d'intégrité, la vérification renvoie `healthy` (sain).
    ```
    curl -X GET http://169.62.196.238/ -H "Host: albhealth.mycluster-12345.us-south.containers.appdomain.cloud"
    ```
    {: pre}

10. Vérifiez que l'adresse IP de l'ALB a été restaurée dans l'enregistrement DNS pour votre domaine en contrôlant le serveur Cloudflare. Notez que la mise à jour de l'enregistrement DNS peut prendre quelques minutes.
    ```
    host mycluster-12345.us-south.containers.appdomain.cloud ada.ns.cloudflare.com
    ```
    {: pre}

    Exemple de sortie :
    ```
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.46.52.222
    mycluster-12345.us-south.containers.appdomain.cloud has address 169.62.196.238
    ```
    {: screen}

<br />


## Aide et assistance
{: #ingress_getting_help}

Vous avez encore des problèmes avec votre cluster ?
{: shortdesc}

-  Dans le terminal, vous êtes averti des mises à jour disponibles pour l'interface de ligne de commande `ibmcloud` et les plug-ins. Veillez à maintenir votre interface de ligne de commande à jour pour pouvoir utiliser l'ensemble des commandes et des indicateurs.
-   Pour voir si {{site.data.keyword.Bluemix_notm}} est disponible, [vérifiez la page Statut d'{{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/status?selected=status).
-   Publiez une question sur le site [{{site.data.keyword.containerlong_notm}} Slack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://ibm-container-service.slack.com).
    Si vous n'utilisez pas un ID IBM pour votre compte {{site.data.keyword.Bluemix_notm}}, [demandez une invitation](https://bxcs-slack-invite.mybluemix.net/) sur ce site Slack.
    {: tip}
-   Consultez les forums pour établir si d'autres utilisateurs ont rencontré le même problème. Lorsque vous utilisez les forums pour poser une question, balisez votre question de sorte que les équipes de développement {{site.data.keyword.Bluemix_notm}} la voient.
    -   Si vous avez des questions d'ordre technique sur le développement ou le déploiement de clusters ou d'applications à l'aide d'{{site.data.keyword.containerlong_notm}}, publiez-les sur le site [Stack Overflow ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) en leur adjoignant les balises `ibm-cloud`, `kubernetes` et `containers`.
    -   Pour toute question sur le service et les instructions de mise en route, utilisez le forum [IBM Developer Answers ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Incluez les balises `ibm-cloud` et `containers`.
    Voir [Comment obtenir de l'aide](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) pour plus d'informations sur l'utilisation des forums.
-   Contactez le support IBM en ouvrant un cas. Pour savoir comment ouvrir un cas de support IBM ou obtenir les niveaux de support et la gravité des cas, voir [Contacter le support](/docs/get-support?topic=get-support-getting-customer-support).
Lorsque vous signalez un problème, incluez l'ID de votre cluster. Pour identifier l'ID du cluster, exécutez la commande `ibmcloud ks clusters`. Vous pouvez également utiliser l'[outil de débogage et de diagnostic d'{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) pour regrouper et exporter des informations pertinentes de votre cluster à partager avec le support IBM.
{: tip}

