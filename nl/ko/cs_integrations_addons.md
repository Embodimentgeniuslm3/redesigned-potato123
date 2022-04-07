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

# 관리 추가 기능을 사용하여 서비스 추가
{: #managed-addons}

관리 추가 기능을 사용하여 오픈 소스 테크놀로지를 클러스터에 빠르게 추가합니다.
{: shortdesc}

**관리 추가 기능은 무엇입니까?** </br>
관리 {{site.data.keyword.containerlong_notm}} 추가 기능은 Istio 또는 Knative와 같이 오픈 소스 기능으로 클러스터를 향상시키는 쉬운 방법입니다. 클러스터에 추가하는 오픈 소스 버전은 IBM에서 테스트하고 {{site.data.keyword.containerlong_notm}}에서 사용하도록 승인됩니다.

**비용 청구 및 지원이 관리 추가 기능에 어떻게 작동합니까?** </br>
관리 추가 기능은 {{site.data.keyword.Bluemix_notm}} 지원 조직으로 완전히 통합됩니다. 관리 추가 기능 사용에 대한 질문이나 문제가 있으면 {{site.data.keyword.containerlong_notm}} 지원 채널 중 하나를 사용할 수 있습니다. 자세한 정보는 [도움 및 지원 받기](/docs/containers?topic=containers-cs_troubleshoot_clusters#clusters_getting_help)를 참조하십시오.

클러스터에 추가하는 도구의 비용이 발생하는 경우 해당 비용이 자동으로 통합되어 {{site.data.keyword.containerlong_notm}} 비용 청구의 일부로 나열됩니다. 비용 청구 주기는 {{site.data.keyword.Bluemix_notm}}에서 결정하며 클러스터에서 추가 기능을 사용으로 설정한 시기에 따라 달라집니다.

**처리하기 위해 필요한 제한사항은 무엇입니까?** </br>
클러스터에 [컨테이너 이미지 보안 적용기 허가 제어기](/docs/services/Registry?topic=registry-security_enforce#security_enforce)가 설치되어 있으면 클러스터에서 관리 추가 기능을 사용할 수 없습니다.

## 관리 추가 기능 추가
{: #adding-managed-add-ons}

클러스터에서 관리 추가 기능을 사용으로 설정하기 위해 [`ibmcloud ks cluster-addon-enable <addon_name> --cluster <cluster_name_or_ID>` 명령](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_cluster_addon_enable)을 사용합니다. 관리 추가 기능을 사용으로 설정하는 경우 모든 Kubernetes 리소스를 포함한 도구의 지원되는 버전이 클러스터에 자동으로 설치됩니다. 클러스터가 관리 추가 기능을 설치하기 위해 충족해야 하는 전제조건을 찾으려면 각 관리 추가 기능의 문서를 참조하십시오.

각 추가 기능과 관련된 전제조건에 대한 정보는 다음을 참조하십시오.

- [Istio](/docs/containers?topic=containers-istio#istio)
- [Knative](/docs/containers?topic=containers-serverless-apps-knative)

## 관리 추가 기능 업데이트
{: #updating-managed-add-ons}

각 관리 추가 기능의 버전은 {{site.data.keyword.Bluemix_notm}}에서 테스트되고 {{site.data.keyword.containerlong_notm}}에서 사용하도록 승인됩니다. 추가 기능의 컴포넌트를 {{site.data.keyword.containerlong_notm}}에서 지원하는 최선 버전으로 업데이트하려면 다음 단계를 따르십시오.
{: shortdesc}

1. 추가 기능이 최신 버전인지 확인하십시오. `* (<version> latest)`로 선언되는 모든 추가 기능은 업데이트될 수 있습니다.
   ```
   ibmcloud ks cluster-addons --cluster <mycluster>
   ```
   {: pre}

   출력 예:
   ```
   OK
   Name      Version
   istio     1.1.5
   knative   0.5.2
   ```
   {: screen}

2. 추가 기능으로 생성된 네임스페이스에서 작성하거나 수정한 리소스(예: 서비스 또는 앱에 대한 구성 파일)를 저장하십시오. 예를 들어, Istio 추가 기능은 `istio-system`을 사용하고 Knative 추가 기능은 `knative-serving`, `knative-monitoring`, `knative-eventing` 및 `knative-build`를 사용합니다.
   명령 예:
   ```
   kubectl get pod <pod_name> -o yaml -n istio-system
   ```
   {: pre}

3. 관리 추가 기능의 네임스페이스에서 자동으로 생성된 Kubernetes 리소스를 로컬 머신의 YAML 파일에 저장하십시오. 이 리소스는 사용자 정의 리소스 정의(CRD)를 사용하여 생성됩니다.
   1. 추가 기능에서 사용하는 네임스페이스에서 추가 기능에 대한 CRD를 가져오십시오. 예를 들어, Istio 추가 기능의 경우 `istio-system` 네임스페이스에서 CRD를 가져오십시오.
      ```
      kubectl get crd -n istio-system
      ```
      {: pre}

   2. 이 CRD에서 작성한 리소스를 저장하십시오.

4. Knative의 선택사항: 다음 리소스 중 하나를 수정한 경우 YAML 파일을 가져와서 로컬 머신에 저장하십시오. 이러한 리소스를 수정했으나 설치된 기본값을 대신 사용하려는 경우에는 리소스를 삭제할 수 있습니다. 몇 분 후에 리소스는 설치된 기본값을 사용하여 다시 작성됩니다.
  <table summary="Knative 리소스 표">
  <caption>Knative 리소스</caption>
  <thead><tr><th>리소스 이름</th><th>리소스 유형</th><th>네임스페이스</th></tr></thead>
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

  명령 예:
  ```
  kubectl get cm config-autoscaler -n knative-serving -o yaml > config-autoscaler.yaml
  ```
  {: pre}

5. `istio-sample-bookinfo` 및 `istio-extras` 추가 기능을 사용으로 설정했던 경우 사용 안함으로 설정하십시오.
   1. `istio-sample-bookinfo` 추가 기능을 사용 안함으로 설정하십시오.
      ```
  ibmcloud ks cluster-addon-disable istio-sample-bookinfo --cluster <cluster_name_or_ID>
      ```
      {: pre}

   2. `istio-extras` 추가 기능을 사용 안함으로 설정하십시오.
      ```
  ibmcloud ks cluster-addon-disable istio-extras --cluster <cluster_name_or_ID>
      ```
      {: pre}

6. 추가 기능을 사용 안함으로 설정하십시오.
   ```
   ibmcloud ks cluster-addon-disable <add-on_name> --cluster <cluster_name_or_ID> -f
   ```
   {: pre}

7. 다음 단계로 진행하기 전에, 추가 기능 네임스페이스 내의 추가 기능 또는 추가 기능 네임스페이스 자체에서 리소스가 제거되었는지 확인하십시오. 
   * 예를 들어, `istio-extras` 추가 기능을 업데이트하는 경우 `grafana`, `kiali` 및 `jaeger-*` 서비스가 `istio-system` 네임스페이스에서 제거되었는지 확인할 수 있습니다.
     ```
     kubectl get svc -n istio-system
     ```
     {: pre}
   * 예를 들어, `knative` 추가 기능을 업데이트하는 경우 `knative-serving`, `knative-monitoring`, `knative-eventing`, `knative-build` 및 `istio-system` 네임스페이스가 삭제되었는지 확인할 수 있습니다. 네임스페이스는 삭제되기 전 몇 분 동안 `Terminating` **상태**일 수 있습니다.
     ```
     kubectl get namespaces -w
     ```
     {: pre}

8. 업데이트할 추가 기능 버전을 선택하십시오.
   ```
   ibmcloud ks addon-versions
   ```
   {: pre}

9. 추가 기능을 사용으로 설정하십시오. `--version` 플래그를 사용하여 설치할 버전을 지정하십시오. 버전이 지정되지 않은 경우, 기본 버전이 설치됩니다.
   ```
   ibmcloud ks cluster-addon-enable <add-on_name> --cluster <cluster_name_or_ID> --version <version>
   ```
   {: pre}

10. 2단계에서 저장한 CRD 리소스를 적용하십시오.
    ```
    kubectl apply -f <file_name> -n <namespace>
    ```
    {: pre}

11. Knative의 경우의 선택사항: 3단계에서 리소스를 저장한 경우에는 이를 다시 적용하십시오.
    명령 예:
    ```
    kubectl apply -f config-autoscaler.yaml -n knative-serving
    ```
    {: pre}

12. Istio의 선택사항: `istio-extras` 및 `istio-sample-bookinfo` 추가 기능을 다시 사용으로 설정하십시오. `istio` 추가 기능의 버전과 동일한 이 추가 기능의 버전을 사용하십시오.
    1. `istio-extras` 추가 기능을 사용으로 설정하십시오.
       ```
       ibmcloud ks cluster-addon-enable istio-extras --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

    2. `istio-sample-bookinfo` 추가 기능을 사용으로 설정하십시오.
       ```
       ibmcloud ks cluster-addon-enable istio-sample-bookinfo --cluster <cluster_name_or_ID> --version <version>
       ```
       {: pre}

13. Istio의 경우의 선택사항: 게이트웨이 구성 파일에서 TLS 섹션을 사용하는 경우에는 Envoy가 시크릿에 액세스할 수 있도록 게이트웨이를 삭제하고 다시 작성해야 합니다. 
  ```
  kubectl delete gateway mygateway
  kubectl create -f mygateway.yaml
  ```
  {: pre}
