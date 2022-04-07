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


# Ajustando o Desempenho
{: #kernel}

Se você tiver requisitos de otimização de desempenho específicos, será possível mudar as configurações padrão para alguns componentes do cluster no {{site.data.keyword.containerlong}}.
{: shortdesc}

Se você optar por mudar as configurações padrão, você estará fazendo isso por sua própria conta e risco. Você é responsável por executar testes com relação a quaisquer configurações mudadas e para quaisquer interrupções potenciais causadas pelas configurações mudadas em seu ambiente.
{: important}

## Otimizando o desempenho do nó do
{: #worker}

Se você tiver requisitos de otimização de desempenho específicos, será possível mudar as configurações padrão para os parâmetros `sysctl` do kernel do Linux em nós do trabalhador.
{: shortdesc}

Os nós do trabalhador são provisionados automaticamente com o desempenho do kernel otimizado, mas é possível mudar as configurações padrão aplicando um objeto `DaemonSet` do [Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) customizado ao seu cluster. O conjunto de daemons altera as configurações para todos os nós do trabalhador existentes e aplica as configurações a todos os novos nós do trabalhador que são fornecidos no cluster. Nenhum pods é afetado.

Você deve ter a [função de serviço IAM do **Manager** {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-users#platform) para todos os namespaces para executar o `initContainer`privilegiado de amostra. Após os contêineres para as implementações serem inicializados, os privilégios serão eliminados.
{: note}

1. Salve o daemon a seguir configurado em um arquivo denominado `worker-node-kernel-settings.yaml`. Na seção `spec.template.spec.initContainers`, inclua os campos e valores para os parâmetros `sysctl` que você deseja ajustar. Esse conjunto de daemons de exemplo muda o número máximo padrão de conexões que são permitidas no ambiente por meio da configuração `net.core.somaxconn` e do intervalo de portas efêmeras por meio da configuração `net.ipv4.ip_local_port_range`.
    ```
    apiVersion: apps/v1
    kind: DaemonSet
    metadata:
      name: kernel-optimization
      namespace: kube-system
      labels:
        tier: management
        app: kernel-optimization
    spec:
      selector:
        matchLabels:
          name: kernel-optimization
      template:
        metadata:
          labels:
            name: kernel-optimization
        spec:
          hostNetwork: true
          hostPID: true
          hostIPC: true
          initContainers:
            - :
                - sh
                - -c
                - sysctl -w net.ipv4.ip_local_port_range="1025 65535"; sysctl -w net.core.somaxconn=32768;
              image: alpine:3.6
              imagePullPolicy: IfNotPresent
              name: sysctl
              resources: {}
              securityContext:
                privileged: true
                capabilities:
                  add:
                    - NET_ADMIN
              volumeMounts:
                - name: modifysys
                  mountPath: /sys
          containers:
            - resources:
                requests:
                  cpu: 0.01
              image: alpine:3.6
              name: sleepforever
              command: ["/bin/sh", "-c"]
              args:
                - >
                  while true; do
                    sleep 100000;
                  done
          volumes:
            - name: modifysys
              hostPath:
                path: /sys
    ```
    {: codeblock}

2. Aplique o conjunto de daemons aos seus nós do trabalhador. As mudanças são aplicadas imediatamente.
    ```
    kubectl apply -f worker-node-node-kernel-settings.yaml
    ```
    {: pre}

<br />

Para reverter os parâmetros `sysctl` de seus nós do trabalhador para os valores padrão configurados pelo {{site.data.keyword.containerlong_notm}}:

1. Exclua o conjunto de daemon. Os `initContainers` que aplicaram as configurações customizadas são removidos.
    ```
    kubectl delete ds kernel-optimization
    ```
    {: pre}

2. [Reinicialize todos os nós do trabalhador no cluster](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reboot). Os nós do trabalhador voltam on-line, com os valores padrão aplicados.

<br />


## Otimizando o desempenho do pod
{: #pod}

Se você tiver demandas de carga de trabalho de desempenho específicas, será possível mudar as configurações padrão para os parâmetros `sysctl` do kernel do Linux em namespaces de rede de pod.
{: shortdesc}

Para otimizar as configurações do kernel para os pods de app, é possível inserir uma correção [`initContainer ` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) no YAML `pod/ds/rs/deployment` para cada implementação. O `initContainer` é incluído em cada implementação de app que está no namespace de rede do pod para o qual você deseja otimizar o desempenho.

Antes de iniciar, assegure-se de que você tenha a [ função de serviço **Gerenciador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para todos os namespaces para executar a amostra privilegiada `initContainer`. Após os contêineres para as implementações serem inicializados, os privilégios serão eliminados.

1. Salve a correção `initContainer` a seguir em um arquivo denominado `pod-patch.yaml` e inclua os campos e valores para os parâmetros `sysctl` que você deseja ajustar. Este exemplo `initContainer` muda o número máximo padrão de conexões permitidas no ambiente por meio da configuração `net.core.somaxconn` e do intervalo de portas efêmeras por meio da configuração `net.ipv4.ip_local_port_range`.
    ```
    spec:
      template:
        spec:
          initContainers:
          - :
            - sh
            - -c
            - sysctl -e -w net.core.somaxconn=32768;  sysctl -e -w net.ipv4.ip_local_port_range="1025 65535";
            image: alpine:3.6
            imagePullPolicy: IfNotPresent
            name: sysctl
            resources: {}
            securityContext:
              privileged: true
    ```
    {: codeblock}

2. Corrigir cada uma de suas implementações.
    ```
    implementação kubectl patch deployment < deployment_name> -- patch pod-patch.yaml
    ```
    {: pre}

3. Se você mudou o valor `net.core.somaxconn` nas configurações do kernel, a maioria dos apps poderá usar automaticamente o valor atualizado. No entanto, alguns apps podem requerer que você mude manualmente o valor correspondente em seu código de app para corresponder ao valor de kernel. Por exemplo, se você está ajustando o desempenho de um pod no qual um app NGINX está sendo executado, deve-se mudar o valor do campo `backlog` no código de app NGINX para corresponder. Para obter mais informações, consulte esta [postagem do blog NGINX ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.nginx.com/blog/tuning-nginx/).

<br />


## Ajustando os recursos do provedor de métricas do cluster
{: #metrics}

As configurações do provedor de métricas do seu cluster (`metrics-server` no Kubernetes 1.12 e mais recente ou `heapster` em versões anteriores) são otimizadas para clusters com 30 ou menos pods por nó do trabalhador. Se o seu cluster tiver mais pods por nó do trabalhador, o provedor de métricas `metrics-server` ou o contêiner principal `heapster` para o pod poderá ser reiniciado frequentemente com uma mensagem de erro, como `OOMKilled`.

O pod do provedor de métricas também tem um contêiner `nanny` que escala as solicitações de recurso do contêiner principal `metrics-server` ou `heapster` e limites em resposta ao número de nós do trabalhador no cluster. É possível mudar os recursos padrão editando o configmap do provedor de métricas.

Antes de iniciar: [Efetue login em sua conta. Se aplicável, direcione o grupo de recursos apropriado. Configure o contexto para o seu cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Abra o YAML do configmap do provedor de métricas do cluster.
    *  Para `metrics-server`:
       ```
       kubectl get configmap metrics-server-config -n kube-system -o yaml
       ```
       {: pre}
    *  Para `heapster`:
       ```
       kubectl get configmap heapster-config -n kube-system -o yaml
       ```
       {: pre}
    Saída de exemplo:
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
    kind: ConfigMap
    metadata:
      annotations:
        armada-service: cruiser-kube-addons
        version: --
      creationTimestamp: 2018-10-09T20:15:32Z
      labels:
        addonmanager.kubernetes.io/mode: EnsureExists
        kubernetes.io/cluster-service: "true"
      name: heapster-config
      namespace: kube-system
      resourceVersion: "526"
      selfLink: /api/v1/namespaces/kube-system/configmaps/heapster-config
      uid: 11a1aaaa-bb22-33c3-4444-5e55e555e555
    ```
    {: screen}

2.  Inclua o campo `memoryPerNode` no configmap na seção `data.NannyConfiguration`. O valor padrão para ambos `metrics-server` e `heapster` está configurado como `4Mi`.
    ```
    apiVersion: v1
    data:
      NannyConfiguration: |-
        apiVersion: nannyconfig/v1alpha1
        kind: NannyConfiguration
        memoryPerNode: 5Mi
    kind: ConfigMap
    ...
    ```
    {: codeblock}

3.  Aplique suas mudanças.
    ```
    kubectl apply -f heapster-config.yaml
    ```
    {: pre}

4.  Monitore os pods do provedor de métricas para ver se os contêineres continuam sendo reinicializados devido a uma mensagem de erro `OOMKilled`. Em caso afirmativo, repita estas etapas e aumente o tamanho `memoryPerNode` até que o pod esteja estável.

Deseja ajustar mais configurações? Confira os [docs de configuração de redimensionador de complemento do Kubernetes ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/kubernetes/autoscaler/tree/master/addon-resizer#addon-resizer-configuration) para obter mais ideias.
{: tip}
