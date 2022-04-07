---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-11"

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


# Configuración de políticas de seguridad de pod
{: #psp}

Con [políticas de seguridad de pod (PSP) ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/), puede configurar políticas para autorizar quién puede crear y actualizar pods en {{site.data.keyword.containerlong}}.

**¿Por qué debo definir políticas de seguridad de pod?**</br>
Como administrador del clúster, desea controlar lo que sucede en el clúster, especialmente las acciones que afectan a la seguridad y a la preparación del clúster. Las políticas de seguridad de pod pueden ayudarle a controlar el uso de contenedores privilegiados, espacios de nombres raíz, host de red y puertos, tipos de volúmenes, sistemas de archivos de host, permisos de Linux como los ID de solo lectura o de grupo, etc.

Con el controlador de admisiones `PodSecurityPolicy`, no se pueden crear pods hasta después de [autorizar políticas](#customize_psp). La configuración de políticas de seguridad de pod puede tener efectos secundarios no deseados, por lo tanto asegúrese de probar un despliegue después de cambiar la política. Para desplegar apps, las cuentas de usuario y de servicio deben estar autorizadas por las políticas de seguridad de pod necesarias para desplegar pods. Por ejemplo, si instala apps utilizando [Helm](/docs/containers?topic=containers-helm#public_helm_install), el componente tiller de Helm crea pods y, por lo tanto, debe tener la autorización de política de seguridad de pod correcta.

¿Está intentando controlar qué usuarios tienen acceso a {{site.data.keyword.containerlong_notm}}? Consulte [Asignación de acceso de clúster](/docs/containers?topic=containers-users#users) para establecer los permisos de {{site.data.keyword.Bluemix_notm}} IAM y de infraestructura.
{: tip}

**¿Hay políticas predeterminadas establecidas? ¿Qué puedo añadir?**</br>
De forma predeterminada, {{site.data.keyword.containerlong_notm}} configura el controlador de admisiones `PodSecurityPolicy` con [recursos para la gestión de clústeres de {{site.data.keyword.IBM_notm}}](#ibm_psp) que no puede suprimir ni modificar. Tampoco puede inhabilitar el controlador de admisiones.

Las acciones de pod no están bloqueadas de forma predeterminada. En su lugar, dos recursos de control de acceso basados en rol (RBAC) del clúster autorizan a todos los administradores, usuarios, servicios y nodos a crear pods privilegiados y sin privilegios. Se incluyen recursos de RBAC adicionales para la portabilidad con paquetes de {{site.data.keyword.Bluemix_notm}} privado que se utilizan para [despliegues híbridos](/docs/containers?topic=containers-hybrid_iks_icp#hybrid_iks_icp).

Si desea evitar que determinados usuarios creen o actualicen pods, puede [modificar estos recursos de RBAC o crear el suyo propio su propio](#customize_psp).

**¿Cómo funciona la autorización de políticas?**</br>
Cuando crea, como usuario, un pod directamente y no utiliza un controlador, como por ejemplo un despliegue, las credenciales se validan con las políticas de seguridad de pod que tiene autorización para utilizar. Si ninguna política da soporte a los requisitos de seguridad de pod, el pod no se crea.

Cuando crea un pod utilizando un controlador de recursos, como por ejemplo un despliegue, Kubernetes valida las credenciales de la cuenta de servicio del pod con las políticas de seguridad de pod que la cuenta de servicio tiene autorización para utilizar. Si ninguna política da soporte a los requisitos de seguridad de pod, el controlador se ejecuta correctamente, pero el pod no se crea.

Para ver los mensajes de error más comunes, consulte [Las pods no se pueden desplegar debido a una política de seguridad de pod](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_psp).

**¿Por qué todavía puedo crear pods con privilegios si no formo parte del enlace de rol de clúster `privileged-psp-user`?**<br>
Otros enlaces de rol de clúster o enlaces de rol limitados a un espacio de nombres pueden proporcionarle otras políticas de seguridad de pod que le autoricen a crear pods con privilegios. Además, de forma predeterminada, los administradores de un clúster tienen acceso a todos los recursos, incluidas las políticas de seguridad de pod, y por lo tanto pueden añadirse a PSP o crear recursos privilegiados.

## Personalización de políticas de seguridad de pod
{: #customize_psp}

Para evitar acciones pod no autorizadas, puede modificar los recursos de política de seguridad de pod existentes o crear el suyo propio. Debe ser administrador del clúster para poder personalizar las políticas.
{: shortdesc}

**¿Cuáles son las políticas existentes que puedo modificar?**</br>
De forma predeterminada, el clúster contiene los siguientes recursos de RBAC que permiten a los administradores de clúster, usuarios autenticados, cuentas de servicio y nodos utilizar las políticas de seguridad de pod `ibm-privileged-psp` e `ibm-restricted-psp`. Estas políticas permiten a los usuarios crear y actualizar pods con privilegios y sin privilegios (restringidos).

| Nombre | Espacio de nombres | Tipo | Finalidad |
|---|---|---|---|
| `privileged-psp-user` | Todos | `ClusterRoleBinding` | Permite a los administradores de clúster, usuarios autenticados, cuentas de servicio y nodos utilizar la política de seguridad de pod `ibm-privileged-psp`. |
| `restricted-psp-user` | Todos | `ClusterRoleBinding` | Permite a los administradores de clúster, usuarios autenticados, cuentas de servicio y nodos utilizar la política de seguridad de pod `ibm-restricted-psp`. |
{: caption="Recursos de RBAC predeterminados que puede modificar" caption-side="top"}

Puede modificar estos roles de RBAC para eliminar o añadir administradores, usuarios, servicios o nodos a la política.

Antes de empezar:
*  [Inicie una sesión en su cuenta. Si procede, apunte al grupo de recursos adecuado. Establezca el contexto para el clúster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
*  Comprende cómo se trabaja con los roles de RBAC. Para obtener más información, consulte [Autorización de usuarios con roles de RBAC personalizados de Kubernetes](/docs/containers?topic=containers-users#rbac) o la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/access-authn-authz/rbac/#api-overview).
* Asegúrese de tener el [rol de **Gestor** de acceso al servicio {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) sobre todos los espacios de nombres.

Si modifica la configuración predeterminada, puede evitar acciones de clúster importantes, como despliegues de pod o actualizaciones de clúster. Pruebe sus cambios en un clúster que no sea de producción y en el que no confíen otros equipos.
{: important}

**Para modificar los recursos de RBAC**:
1.  Obtenga el nombre del enlace de rol de clúster de RBAC.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Descargue el enlace de rol de clúster como un archivo `.yaml` que puede editar localmente.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml > privileged-psp-user.yaml
    ```
    {: pre}

    Quizás desee guardar una copia de la política existente para poder recuperarla si la política modificada no ofrece los resultados esperados.
    {: tip}

    **Ejemplo de archivo de enlace de rol de clúster**:

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

3.  Edite el archivo `.yaml` de enlace de rol de clúster. Para saber lo que puede editar, revise la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/). Acciones de ejemplo:

    *   **Cuentas de servicio**: es posible que desee autorizar cuentas de servicio para que los despliegues solio se puedan producir en espacios de nombres específicos. Por ejemplo, si limita el ámbito de la política para permitir acciones dentro del espacio de nombres `kube-system`, se pueden producir muchas acciones importantes como, por ejemplo, actualizaciones de clúster. Sin embargo, las acciones en otros espacios de nombres dejan de estar autorizadas.

        Para dejar que la política permita acciones en un espacio de nombres específico, cambie el valor `system:serviceaccounts` por `system:serviceaccount:<namespace>`.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:serviceaccount:kube-system
        ```
        {: codeblock}

    *   **Usuarios**: es posible que desee eliminar la autorización para todos los usuarios autenticados para desplegar pods con acceso privilegiado. Elimine la siguiente entrada `system:authenticated`.
        ```yaml
        - apiGroup: rbac.authorization.k8s.io
          kind: Group
          name: system:authenticated
        ```
        {: codeblock}

4.  Cree el recurso de enlace de rol de clúster modificado en el clúster.

    ```
    kubectl apply -f privileged-psp-user.yaml
    ```
    {: pre}

5.  Verifique que el recurso se ha modificado.

    ```
    kubectl get clusterrolebinding privileged-psp-user -o yaml
    ```
    {: pre}

</br>
**Para suprimir los recursos de RBAC**:
1.  Obtenga el nombre del enlace de rol de clúster de RBAC.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

2.  Suprima el rol de RBAC que desea eliminar.
    ```
    kubectl delete clusterrolebinding privileged-psp-user
    ```
    {: pre}

3.  Verifique que el enlace de rol de clúster de RBAC ya no está en el clúster.
    ```
    kubectl get clusterrolebinding
    ```
    {: pre}

</br>
**Para crear su propia política de seguridad de pod**:</br>
Para crear su propio recurso de política de seguridad de pod y autorizar a los usuarios con RBAC, revise la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/concepts/policy/pod-security-policy/).

Asegúrese de que ha modificado las políticas existentes de modo que la nueva política que cree no entre en conflicto con la política existente. Por ejemplo, la política existente permite a los usuarios crear y actualizar pods privilegiados. Si crea una política que no permite a los usuarios crear o actualizar pods privilegiados, el conflicto entre la política existente y la nueva podría ocasionar resultados inesperados.

## Visión general de los recursos predeterminados para la gestión de clústeres de {{site.data.keyword.IBM_notm}}
{: #ibm_psp}

El clúster de Kubernetes en {{site.data.keyword.containerlong_notm}} contiene las siguientes políticas de seguridad de pod y los recursos de RBAC relacionados para permitir que {{site.data.keyword.IBM_notm}} gestione correctamente el clúster.
{: shortdesc}

Los recursos de RBAC predeterminados `PodSecurityPolicy` hacen referencia a las políticas de seguridad de pod definidas por {{site.data.keyword.IBM_notm}}.

**Atención**: no debe suprimir ni modificar estos recursos.

| Nombre | Espacio de nombres | Tipo | Finalidad |
|---|---|---|---|
| `ibm-anyuid-hostaccess-psp` | Todos | `PodSecurityPolicy` | Política para la creación de pod de acceso de host completo. |
| `ibm-anyuid-hostaccess-psp-user` | Todos | `ClusterRole` | Rol de clúster que permite el uso de la política de seguridad de pod `ibm-anyuid-hostaccess-psp`. |
| `ibm-anyuid-hostpath-psp` | Todos | `PodSecurityPolicy` | Política para la creación de pod de acceso de host. |
| `ibm-anyuid-hostpath-psp-user` | Todos | `ClusterRole` | Rol de clúster que permite el uso de la política de seguridad de pod `ibm-anyuid-hostpath-psp`. |
| `ibm-anyuid-psp` | Todos | `PodSecurityPolicy` | Política para la creación de cualquier pod ejecutable de UID/GID. |
| `ibm-anyuid-psp-user` | Todos | `ClusterRole` | Rol de clúster que permite el uso de la política de seguridad de pod `ibm-anyuid-psp`. |
| `ibm-privileged-psp` | Todos | `PodSecurityPolicy` | Política para la creación de pod con privilegios. |
| `ibm-privileged-psp-user` | Todos | `ClusterRole` | Rol de clúster que permite el uso de la política de seguridad de pod `ibm-privileged-psp`. |
| `ibm-privileged-psp-user` | `kube-system` | `RoleBinding` | Permite a los administradores de clúster, cuentas de servicio y nodos utilizar la política de seguridad de pod `ibm-privileged-psp` en el espacio de nombres `kube-system`. |
| `ibm-privileged-psp-user` | `ibm-system` | `RoleBinding` | Permite a los administradores de clúster, cuentas de servicio y nodos utilizar la política de seguridad de pod `ibm-privileged-psp` en el espacio de nombres `ibm-system`. |
| `ibm-privileged-psp-user` | `kubx-cit` | `RoleBinding` | Permite a los administradores de clúster, cuentas de servicio y nodos utilizar la política de seguridad de pod `ibm-privileged-psp` en el espacio de nombres `kubx-cit`. |
| `ibm-restricted-psp` | Todos | `PodSecurityPolicy` | Política para la creación de pod sin privilegios o restringida. |
| `ibm-restricted-psp-user` | Todos | `ClusterRole` | Rol de clúster que permite el uso de la política de seguridad de pod `ibm-restricted-psp`. |
{: caption="Recursos de políticas de seguridad de pod de IBM que no debe modificar" caption-side="top"}
