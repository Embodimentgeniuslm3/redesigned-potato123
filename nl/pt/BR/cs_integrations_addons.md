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

# Incluindo serviços com o uso de complementos gerenciados
{: #managed-addons}

Inclua rapidamente tecnologias de software livre em seu cluster com complementos gerenciados.
{: shortdesc}

**O que são complementos gerenciados?**</br>
Os complementos gerenciados do {{site.data.keyword.containerlong_notm}} são uma maneira fácil de aprimorar seu cluster com recursos de software livre, como o Istio ou o Knative. A versão da ferramenta de software livre incluída em seu cluster é testada pela IBM e aprovada para ser usada no {{site.data.keyword.containerlong_notm}}.

**Como o faturamento e o suporte funcionam para complementos gerenciados?**</br>
Os complementos gerenciados são totalmente integrados na organização de suporte do {{site.data.keyword.Bluemix_notm}}. Se tiver uma pergunta ou um problema com o uso dos complementos gerenciados, será possível usar um dos canais de suporte do {{site.data.keyword.containerlong_notm}}. Para obter mais informações, consulte [Obtendo ajuda e suporte](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help).

Se a ferramenta incluída em seu cluster incorrer em custos, eles serão integrados automaticamente e listados como parte de seu faturamento do {{site.data.keyword.containerlong_notm}}. O ciclo de faturamento é determinado pelo {{site.data.keyword.Bluemix_notm}}, dependendo de quando você ativou o complemento em seu cluster.

**Quais limitações preciso considerar?**</br>
Se você instalou o [controlador de admissão de imposição de segurança de imagem do contêiner](/docs/services/Registry?topic=registry-security_enforce#security_enforce) em seu cluster, não será possível ativar nele os complementos gerenciados.

## Incluindo complementos gerenciados
{: #adding-managed-add-ons}

Para ativar um complemento gerenciado em seu cluster, você usa o comando [`ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>`](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable). Quando você ativa o complemento gerenciado, uma versão suportada da ferramenta, incluindo todos os recursos do Kubernetes, é automaticamente instalada em seu cluster. Consulte a documentação de cada complemento gerenciado para localizar os pré-requisitos que seu cluster deve atender para instalar o complemento gerenciado.

Para obter mais informações sobre os pré-requisitos de cada complemento, consulte:

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## Atualizando complementos gerenciados
{: #updating-managed-add-ons}

As versões de cada complemento gerenciado são testadas pelo {{site.data.keyword.Bluemix_notm}} e aprovadas para uso no {{site.data.keyword.containerlong_notm}}. Para atualizar os componentes de um complemento para a versão mais recente suportada pelo {{site.data.keyword.containerlong_notm}}, use as etapas a seguir.
{: shortdesc}

1. Verifique se seus complementos estão na versão mais recente. Quaisquer complementos denotados com `* (<version> latest)` podem ser atualizados.
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   Saída de exemplo:
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. Salve todos os recursos, como arquivos de configuração para quaisquer serviços ou aplicativos, criados ou modificados no namespace gerado pelo complemento. Por exemplo, o complemento do Istio usa `istio-system` e o complemento do Knative usa `knative-serving`, `knative-monitoring`, `knative-eventing` e `knative-build`.
   Exemplo de comando:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. Salve os recursos do Kubernetes gerados automaticamente no namespace do complemento gerenciado em um arquivo YAML em sua máquina local. Esses recursos são gerados usando definições de recurso customizadas (CRDs).
   1. Obtenha os CRDs para seu complemento do namespace usado pelo seu complemento. Por exemplo, para o complemento do Istio, obtenha os CRDs do namespace `istio-system`.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. Salve qualquer recurso criado por meio desses CRDs.

4. Opcional para Knative: se você modificou qualquer um dos recursos a seguir, obtenha o arquivo YAML e salve-os em sua máquina local. Se você modificou qualquer um desses recursos, mas deseja usar o padrão instalado no lugar, é possível excluir o recurso. Após alguns minutos, o recurso é recriado com os valores padrão instalados.
  <table summary="Tabela de recursos do Knative">
  <caption>Recursos do Knative</caption>
  <thead><tr><th>Nome do recurso</th><th>Tipo de recurso</th><th>Namespace</th></tr></thead>
  <tbody>
  <tr><td><code>config-autoscaler</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-controller</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-domain</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-gc</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-istio</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-logging</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-network</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>config-observability</code></td><td>ConfigMap</td><td><code>knative-serving</code></td></tr>
  <tr><td><code>kaniko</code></td><td>BuildTemplate</td><td><code>padrão</code></td></tr>
  <tr><td><code>iks-knative-ingress</code></td><td>Entrada</td><td><code>istio-system</code></td></tr>
  </tbody></table>

  Exemplo de comando:
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. Se você ativou os complementos `istio-sample-bookinfo` e `istio-extras`, desative-os.
   1. Desative o complemento  ` istio-sample-bookinfo ` .
      ```
      ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. Desative o complemento  ` istio-extras ` .
      ```
      ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. Desative o complemento.
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. Antes de continuar com a próxima etapa, verifique se os recursos do complemento dentro dos namespaces do complemento ou os próprios namespaces do complemento são removidos.
   * Por exemplo, se você atualizar o complemento `istio-extras`, será possível verificar se os serviços `grafana`, `kiali` e `jaeger-*` foram removidos do namespace `istio-system`.
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * Por exemplo, se você atualizar o complemento `knative`, será possível verificar se os namespaces `knative-serving`, `knative-monitoring`, `knative-eventing`, `knative-build` e `istio-system` foram excluídos. Os namespaces podem ter um **STATUS** de `Terminating` por alguns minutos antes de serem excluídos.
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. Escolha a versão do complemento para a qual deseja atualizar.
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. Ative o complemento novamente. Use o sinalizador `--version` para especificar a versão que deseja instalar. Se nenhuma versão for especificada, a versão padrão será instalada.
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. Aplique os recursos de CRD salvos na etapa 2.
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. Opcional para Knative: se você salvou qualquer um dos recursos na etapa 3, reaplique-os.
    Exemplo de comando:
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Opcional para Istio: ative novamente os complementos `istio-extras` e `istio-sample-bookinfo`. Use para esses complementos a mesma versão usada para o complemento `istio`.
    1. Ative o complemento `istio-extras`.
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. Ative o complemento `istio-sample-bookinfo`.
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. Opcional para Istio: se você usa as seções TLS em seus arquivos de configuração de gateway, deve-se excluir e recriar os gateways para que o Envoy possa acessar os segredos.
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
