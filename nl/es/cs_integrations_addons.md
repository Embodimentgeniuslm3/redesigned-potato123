---

copyright:
  years: 2014, 2019
lastupdated: "2019-06-12"

keywords: kubernetes, iks, helm

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

# Adición de servicios utilizando complementos gestionados
{: #managed-addons}

Puede añadir rápidamente tecnologías de código abierto a su clúster con complementos gestionados.
{: shortdesc}

**¿Qué son los complementos gestionados?** </br>
Los complementos gestionados de {{site.data.keyword.containerlong_notm}} son una forma sencilla de mejorar el clúster con prestaciones de código abierto, como por ejemplo, Istio o Knative. La versión de la herramienta de código abierto que añade al clúster se prueba en IBM y se aprueba para ser utilizada en {{site.data.keyword.containerlong_notm}}.

**¿Cómo funciona la facturación y el soporte para los complementos gestionados?** </br>
Los complementos gestionados se integran completamente en la organización de soporte de {{site.data.keyword.Bluemix_notm}}. Si tiene alguna pregunta o algún problema con el uso de los complementos gestionados, puede utilizar uno de los canales de soporte de {{site.data.keyword.containerlong_notm}}. Para obtener más información, consulte [Obtención de ayuda y soporte](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Si la herramienta que añade a su clúster implica costes, estos costes se integran automáticamente y se listan como parte de la facturación de {{site.data.keyword.containerlong_notm}}. {{site.data.keyword.Bluemix_notm}} determina el ciclo de facturación según cuándo haya habilitado el complemento en el clúster.

**¿Qué limitaciones debo considerar?** </br>
Si ha instalado el [controlador de admisiones del gestor de seguridad de imágenes de contenedor](/docs/services/Registry?topic=registry-security_enforce#security_enforce) en el clúster, no puede habilitar los complementos gestionados en el clúster.

## Adición de complementos gestionados
{: #adding-managed-add-ons}

Para habilitar un complemento gestionado en el clúster, utilice el [mandato `ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable). Cuando se habilita el complemento gestionado, se instala automáticamente en el clúster una versión soportada de la herramienta, incluyendo todos los recursos de Kubernetes. Consulte la documentación de cada complemento gestionado para encontrar los requisitos previos que debe cumplir el clúster para poder instalar el complemento gestionado.

Para obtener más información sobre los requisitos previos de cada complemento, consulte:

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## Actualización de complementos gestionados
{: #updating-managed-add-ons}

Las versiones de cada complemento gestionado se prueban en {{site.data.keyword.Bluemix_notm}} y se aprueban para su uso en {{site.data.keyword.containerlong_notm}}. Para actualizar los componentes de un complemento a la versión más reciente soportada por {{site.data.keyword.containerlong_notm}}, siga los pasos siguientes.
{: shortdesc}

1. Compruebe si tiene la última versión de los complementos. Se pueden actualizar todos los complementos marcados con `* (<version> latest)`.
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   Salida de ejemplo:
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. Guarde todos los recursos, como por ejemplo los archivos de configuración de un servicio o app, que haya creado o modificado en el espacio de nombres que ha generado el complemento. Por ejemplo, el complemento Istio utiliza `istio-system` y el complemento Knative utiliza `knative-serving`, `knative-monitoring`, `knative-eventing` y `knative-build`.
   Mandato de ejemplo:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. Guarde los recursos de Kubernetes que se han generado automáticamente en el espacio de nombres del complemento gestionado en un archivo YAML de la máquina local. Estos recursos se generan utilizando definiciones de recursos personalizados (CRD).
   1. Obtenga los CRD del complemento en el espacio de nombres que utiliza el complemento. Por ejemplo, para el complemento Istio, obtenga los CRD del espacio de nombres `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Guarde los recursos creados a partir de estos CRD.

4. Opcional para Knative: Si ha modificado alguno de los siguientes recursos, obtenga el archivo YAML y guárdelo en la máquina local. Si ha modificado alguno de estos recursos, pero desea utilizar en su lugar el valor predeterminado instalado, puede suprimir el recurso. Tras unos pocos minutos, el recurso se vuelve a crear con los valores predeterminados instalados.
  <table summary="Tabla de recursos de Knative">
  <caption>Recursos de Knative</caption>
  <thead><tr><th>Nombre de recurso</th><th>Tipo de recurso</th><th>Espacio de nombres</th></tr></thead>
  <tbody>
  <tr><td><code>config-autoscaler</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-controller</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-domain</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-gc</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-istio</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-logging</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-network</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-observability</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>kaniko</code></td><td>BuildTemplate</td><td><code>default</code></td></tr>
  <tr><td><code>iks-knative-ingress</code></td><td>Ingress</td><td><code>istio-system</code></td></tr>
  </tbody></table>

  Mandato de ejemplo:
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. Si ha habilitado los complementos `istio-sample-bookinfo` e `istio-extras`, inhabilítelos.
   1. Inhabilite el complemento `istio-sample-bookinfo`.
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. Inhabilite el complemento `istio-extras`.
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. Inhabilite el complemento.
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. Antes de continuar con el paso siguiente, verifique que se han eliminado los recursos del complemento dentro de los espacios de nombres del complemento o bien que se han eliminado los espacios de nombres.
   * Por ejemplo, si actualiza el complemento `istio-extras`, puede verificar que se han eliminado los servicios `grafana`, `kiali` y `jaeger-*` del espacio de nombres `istio-system`.
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * Por ejemplo, si actualiza el complemento `knative`, puede comprobar que se han suprimido los espacios de nombres `knative-serving`, `knative-monitoring`, `knative-eventing`, `knative-build` e `istio-system`. Los espacios de nombres pueden tener el **STATUS** `Terminating` durante unos minutos antes de que se supriman.
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. Elija la versión de complemento a la que desea actualizar.
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. Vuelva a habilitar el complemento. Utilice el distintivo `--version` para especificar la versión que desea instalar. Si no se especifica ninguna versión, se instala la versión predeterminada.
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. Aplique los recursos de CRD que ha guardado en el paso 2.
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. Opcional para Knative: Si ha guardado algún recurso o recursos en el paso 3, vuelva a aplicarlos.
    Mandato de ejemplo:
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Opcional para Istio: Vuelva a habilitar los complementos `istio-extras` e `istio-sample-bookinfo`. Utilice la misma versión para estos complementos que para el complemento `istio`.
    1. Habilite el complemento `istio-extras`.
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. Habilite el complemento `istio-sample-bookinfo`.
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. Opcional para Istio: si utiliza secciones de TLS en los archivos de configuración de pasarela, debe suprimir y volver a crear las pasarelas para que Envoy pueda acceder a los secretos.
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
