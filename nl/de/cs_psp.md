---

copyright:
  years: 2014, 2019
lastupdated: "2019-04-03"

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

# Pod-Sicherheitsrichtlinien konfigurieren
{: #psp}

Mit [Pod-Sicherheitsrichtlinien ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/) können Sie
Richtlinien konfigurieren, um anzugeben, wer Pods in {{site.data.keyword.containerlong}} erstellen und aktualisieren darf.

**Warum lege ich Pod-Sicherheitsrichtlinien fest?**</br>
Als Clusteradministrator möchten Sie steuern, was in Ihrem Cluster passiert, insbesondere Aktionen, die sich auf die Sicherheit oder Bereitschaft des Clusters auswirken. Pod-Sicherheitsrichtlinien können Ihnen dabei helfen, die Verwendung von berechtigten Containern, Stammnamensbereichen, Host-Netzbetrieb und -Ports, Datenträgertypen, Hostdateisystemen, Linux-Berechtigungen wie z. B. Nur-Lese-Berechtigung oder Gruppen-IDs und vieles mehr zu steuern.

Mit dem Zugangscontroller `PodSecurityPolicy` können Pods erst nach der [Autorisierung von Richtlinien](#customize_psp) erstellt werden. Die Konfiguration von Pod-Sicherheitsrichtlinien kann unbeabsichtigte Nebeneffekte haben. Deshalb sollten Sie eine Implementierung nach dem Ändern der Richtlinie testen. Um Apps bereitstellen zu können, müssen die entsprechenden Benutzer- und Servicekonten alle durch die Pod-Sicherheitsrichtlinien autorisiert sein, die für die Bereitstellung von Pods erforderlich sind. Wenn Sie beispielsweise Apps mithilfe von [Helm](/docs/containers?topic=containers-helm#public_helm_install) installieren, werden durch die Tiller-Komponente von Helm Pods erstellt. Sie müssen daher über die Autorisierung der richtigen Pod-Sicherheitsrichtlinie verfügen.

Sie möchten steuern, welche Benutzer Zugriff auf {{site.data.keyword.containerlong_notm}} haben? Informationen zum Festlegen von {{site.data.keyword.Bluemix_notm}} IAM- und Infrastrukturberechtigungen finden Sie im Abschnitt [Clusterzugriff zuweisen](/docs/containers?topic=containers-users#users).
{: tip}

**Gibt es Richtlinien, die standardmäßig festgelegt werden? Was kann ich hinzufügen?**</br>
Standardmäßig konfiguriert {{site.data.keyword.containerlong_notm}} den Zugangscontroller `PodSecurityPolicy` mit [Ressourcen für die {{site.data.keyword.IBM_notm}} Clusterverwaltung](#ibm_psp), den Sie nicht löschen oder ändern können. Sie können den Zugangscontroller auch nicht inaktivieren.

Pod-Aktionen sind standardmäßig nicht gesperrt. Stattdessen autorisieren zwei rollenbasierte Zugriffssteuerungsressourcen (RBAC, Role-Based Access Control) im Cluster alle Administratoren, Benutzer, Services und Knoten, um privilegierte und nicht privilegierte Pods zu erstellen. Zusätzliche RBAC-Ressourcen sind für die Portierbarkeit mit privaten {{site.data.keyword.Bluemix_notm}}-Paketen eingeschlossen, die für [Hybridbereitstellungen](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp) verwendet werden.

Wenn Sie bestimmte Benutzer daran hindern möchten, Pods zu erstellen oder zu aktualisieren, können Sie [diese RBAC-Ressourcen ändern oder eigene Ressourcen erstellen](#customize_psp).

**Wie funktioniert die Richtlinienautorisierung?**</br>
Wenn Sie als Benutzer einen Pod direkt erstellen und nicht über einen Controller (z. B. eine Bereitstellung), werden Ihre Berechtigungsnachweise anhand der Pod-Sicherheitsrichtlinien, die Sie verwenden können, geprüft. Wenn keine Richtlinie die Pod-Sicherheitsanforderungen unterstützt, wird der Pod nicht erstellt.

Wenn Sie einen Pod mithilfe eines Ressourcencontrollers erstellen (z. B. eine Bereitstellung), prüft Kubernetes die Berechtigungsnachweise für das Servicekonto des Pods anhand der Pod-Sicherheitsrichtlinien, die das Servicekonto verwenden darf. Wenn keine Richtlinie die Pod-Sicherheitsanforderungen unterstützt, ist der Controller zwar erfolgreich, der Pod wird jedoch nicht erstellt.

Informationen zu allgemeinen Fehlernachrichten finden Sie im Abschnitt [Pods können wegen einer Pod-Sicherheitsrichtlinie nicht bereitgestellt werden](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_psp).

## Pod-Sicherheitsrichtlinien anpassen
{: #customize_psp}

Um unbefugte Pod-Aktionen zu verhindern, können Sie vorhandene Pod-Sicherheitsrichtlinienressourcen ändern oder eigene Ressourcen erstellen. Sie müssen ein Clusteradministrator sein, um Richtlinien anzupassen.
{: shortdesc}

**Welche vorhandenen Richtlinien kann ich ändern?**</br>
Ihr Cluster enthält standardmäßig die folgenden RBAC-Ressourcen, mit denen
Clusteradministratoren, authentifizierte Benutzer, Servicekonten und Knoten die
Sicherheitsrichtlinien `ibm-privileged-psp` und `ibm-restricted-psp` verwenden können. Mit diesen
Richtlinien können Benutzer privilegierte und nicht privilegierte (eingeschränkte) Pods erstellen und aktualisieren.

| Name | Namensbereich | Typ | Zweck |
|---|---|---|---|
| `privileged-psp-user` | cluster-wide | `ClusterRoleBinding` | Ermöglicht Clusteradministratoren, authentifizierten Benutzern, Servicekonten und Knoten die Verwendung der Sicherheitsrichtlinie `ibm-privileged-psp`. |
| `restricted-psp-user` | cluster-wide | `ClusterRoleBinding` | Ermöglicht Clusteradministratoren, authentifizierten Benutzern, Servicekonten und Knoten die Verwendung der Sicherheitsrichtlinie `ibm-restricted-psp`. |
{: caption="Standard-RBAC-Ressourcen, die Sie ändern können" caption-side="top"}

Sie können diese RBAC-Rollen ändern, um Administratoren, Benutzer, Services oder Knoten zu der Richtlinie hinzuzufügen oder aus ihr zu entfernen.

Vorbereitende Schritte:
*  [Melden Sie sich an Ihrem Konto an. Geben Sie als Ziel die entsprechende Region und, sofern zutreffend, die Ressourcengruppe an. Legen Sie den Kontext für den Cluster fest.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  Verstehen Sie die Funktionsweise von RBAC-Rollen. Weitere Informationen hierzu finden Sie im Abschnitt [Benutzer mit angepassten Kubernetes-RBAC-Rollen berechtigen](/docs/containers?topic=containers-users#rbac) oder in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview).
* Stellen Sie sicher, dass Sie über die [{{site.data.keyword.Bluemix_notm}} IAM-Servicezugriffsrolle **Manager**](/docs/containers?topic=containers-users#platform) für alle Namensbereiche verfügen.

Wenn Sie die Standardkonfiguration ändern, können Sie wichtige Clusteraktionen, wie Podbereitstellungen oder Clusteraktualisierungen, verhindern. Testen Sie Ihre Änderungen in einem Cluster, der sich nicht in einer Produktionsumgebung befindet und für andere Teams nicht wichtig ist.
{: important}

**Gehen Sie wie folgt vor, um die RBAC-Ressourcen zu ändern**:
1.  Rufen Sie den Namen der RBAC-Clusterrollenbindung ab.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Laden Sie die Clusterrollenbindung als eine `.yaml`-Datei herunter, die Sie lokal bearbeiten können.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}

    Sie können eine Kopie der vorhandenen Richtlinie speichern, damit Sie zu dieser zurückkehren können, falls die geänderte Richtlinie zu unerwarteten Ergebnissen führt.
    {: tip}

    **Beispiel für eine Clusterrollenbindungsdatei**:

    ```yaml
    apiVersion: rbac.authorization.k8s.io/v1
    kind: ClusterRoleBinding
    metadata:
      creationTimestamp: 2018-06-20T21:19:50Z
      name: privileged-psp-user
      resourceVersion: "1234567"
      selfLink: /apis/rbac.authorization.k8s.io/v1/clusterrolebindings/privileged-psp-user
      uid: aa1a1111-11aa-11a1-aaaa-1a1111aa11a1
    roleRef:
      apiGroup: rbac.authorization.k8s.io
      kind: ClusterRole
      name: ibm-privileged-psp-user
    subjects:
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:masters
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:nodes
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:serviceaccounts
    - apiGroup: rbac.authorization.k8s.io
      kind: Group
      name: system:authenticated
    ```
    {: codeblock}

3.  Bearbeiten Sie die `.yaml`-Datei der Clusterrollenbindung. Informationen darüber, was Sie bearbeiten können, finden Sie in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/). Beispielaktionen:

    *   **Servicekonten**: Sie können Servicekonten autorisieren, damit Bereitstellungen nur in bestimmten Namensbereichen ausgeführt werden können. Wenn Sie z. B. die Richtlinie so definieren, dass Aktionen innerhalb des Namensbereichs `kube-system` zulässig sind, können viele wichtige Aktionen, wie Clusteraktualisierungen, durchgeführt werden. Aktionen in anderen Namensbereichen sind jedoch nicht mehr autorisiert.

        Um die Richtlinie so definieren, dass Aktionen in einem bestimmten Namensbereich zulässig sind, ändern Sie `system:serviceaccounts` in `system:serviceaccount:<namespace>`.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}

    *   **Benutzer**: Sie möchten beispielsweise die Berechtigung für alle authentifizierten Benutzer entfernen, um Pods mit privilegiertem Zugriff bereitzustellen. Entfernen Sie den folgenden Eintrag des Typs `system:authenticated`.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
        ```
        {: codeblock}

4.  Erstellen Sie die geänderte Ressource für die Clusterrollenbindung in Ihrem Cluster.

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}

5.  Überprüfen Sie, ob die Ressource geändert wurde.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
**Gehen Sie wie folgt vor, um die RBAC-Ressource zu löschen**:
1.  Rufen Sie den Namen der RBAC-Clusterrollenbindung ab.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Löschen Sie die RBAC-Rolle, die Sie entfernen möchten.
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}

3.  Stellen Sie sicher, dass die RBAC-Clusterrollenbindung nicht mehr in Ihrem Cluster enthalten ist.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**Vorgehensweise zur Erstellung einer eigenen Pod-Sicherheitsrichtlinie**:</br>
Informationen darüber, wie Sie Ihre eigene Ressource für die Pod-Sicherheitsrichtlinie erstellen und Benutzer mit RBAC berechtigen, finden Sie in der [Kubernetes-Dokumentation ![Symbol für externen Link](../icons/launch-glyph.svg "Symbol für externen Link")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/).

Stellen Sie sicher, dass Sie die vorhandenen Richtlinien so geändert haben, dass die neue Richtlinie, die Sie erstellen, nicht mit der vorhandenen Richtlinie in Konflikt steht. Die vorhandene Richtlinie ermöglicht es Benutzern beispielsweise, privilegierte Pods zu erstellen und zu aktualisieren. Wenn Sie eine Richtlinie erstellen, die es Benutzern nicht erlaubt, privilegierte Pods zu erstellen oder zu aktualisieren, kann der Konflikt zwischen der vorhandenen und der neuen Richtlinie zu unerwarteten Ergebnissen führen.

## Informationen zu Standardressourcen für die {{site.data.keyword.IBM_notm}} Clusterverwaltung
{: #ibm_psp}

Ihr Kubernetes-Cluster in {{site.data.keyword.containerlong_notm}} enthält die folgenden
Pod-Sicherheitsrichtlinien und zugehörigen RBAC-Ressourcen, damit {{site.data.keyword.IBM_notm}} Ihren Cluster ordnungsgemäß verwalten kann.
{: shortdesc}

Die `PodSecurityPolicy`-Standardressourcen beziehen sich auf die von {{site.data.keyword.IBM_notm}} festgelegten Pod-Sicherheitsrichtlinien.

**Achtung**: Sie dürfen diese Ressourcen nicht löschen oder ändern.

| Name | Namensbereich | Typ | Zweck |
|---|---|---|---|
| `ibm-anyuid-hostaccess-psp` | cluster-wide | `PodSecurityPolicy` | Richtlinie für die Pod-Erstellung mit umfassendem Hostzugriff. |
| `ibm-anyuid-hostaccess-psp-user` | cluster-wide | `ClusterRole` | Clusterrolle, die die Verwendung der Pod-Sicherheitsrichtlinie `ibm-anyuid-hostaccess-psp` ermöglicht. |
| `ibm-anyuid-hostpath-psp` | cluster-wide | `PodSecurityPolicy` | Richtlinie für die Pod-Erstellung mit Hostpfadzugriff. |
| `ibm-anyuid-hostpath-psp-user` | cluster-wide | `ClusterRole` | Clusterrolle, die die Verwendung der Pod-Sicherheitsrichtlinie `ibm-anyuid-hostpath-psp` ermöglicht. |
| `ibm-anyuid-psp` | cluster-wide | `PodSecurityPolicy` | Richtlinie für die über UID/GUID ausführbare Pod-Erstellung. |
| `ibm-anyuid-psp-user` | cluster-wide | `ClusterRole` | Clusterrolle, die die Verwendung der Pod-Sicherheitsrichtlinie `ibm-anyuid-psp` ermöglicht. |
| `ibm-privileged-psp` | cluster-wide | `PodSecurityPolicy` | Richtlinie für die privilegierte Pod-Erstellung. |
| `ibm-privileged-psp-user` | cluster-wide | `ClusterRole` | Clusterrolle, die die Verwendung der Sicherheitsrichtlinie `ibm-privileged-psp` ermöglicht. |
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` | Ermöglicht Clusteradministratoren, Servicekonten und Knoten die Verwendung der Sicherheitsrichtlinie `ibm-privileged-psp` im Namensbereich `kube-system`. |
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` | Ermöglicht Clusteradministratoren, Servicekonten und Knoten die Verwendung der Sicherheitsrichtlinie `ibm-privileged-psp` im Namensbereich `ibm-system`. |
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` | Ermöglicht Clusteradministratoren, Servicekonten und Knoten die Verwendung der Sicherheitsrichtlinie `ibm-privileged-psp` im Namensbereich `kubx-cit`. |
| `ibm-restricted-psp` | cluster-wide | `PodSecurityPolicy` | Richtlinie für nicht privilegierte oder eingeschränkte Pod-Erstellung. |
| `ibm-restricted-psp-user` | cluster-wide | `ClusterRole` | Clusterrolle, die die Verwendung der Sicherheitsrichtlinie `ibm-restricted-psp` ermöglicht. |
{: caption="Ressourcen für IBM Pod-Sicherheitsrichtlinien, die Sie nicht ändern dürfen" caption-side="top"}
