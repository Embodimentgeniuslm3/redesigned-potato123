---

copyright:
  years: 2014, 2019
lastupdated: "2019-05-31"

keywords: kubernetes, iks, ingress

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



# Customizando o Ingresso com Anotações
{: #ingress_annotation}

Para incluir recursos em seu application load balancer (ALB) de Ingresso, é possível especificar anotações como metadados em um recurso de Ingresso.
{: shortdesc}

Antes de usar anotações, certifique-se de ter definido adequadamente sua configuração de serviço do Ingress seguindo as etapas em [Balanceamento de carga HTTPS com balanceadores de carga do aplicativo (ALB) Ingress](/docs/containers?topic=containers-ingress). Assim que você tiver configurado o ALB do Ingress com uma configuração básica, será possível expandir os seus recursos incluindo anotações no arquivo do recurso Ingresso.
{: note}

<table>
<caption>Anotações gerais</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotações gerais</th>
<th>Nome</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors"> Ações de erro customizadas </a></td>
<td><code> custom-errors, custom-error-actions </code></td>
<td>Indique as ações customizadas que o ALB pode tomar para erros de HTTP específicos.</td>
</tr>
<tr>
<td><a href="#location-snippets"> Snippets de local </a></td>
<td><code> location-snippets </code></td>
<td>Inclua uma configuração de bloco de local customizado para um serviço.</td>
</tr>
<tr>
<td><a href="#alb-id">Roteamento de ALB privado</a></td>
<td><code>ALB-ID</code></td>
<td>Roteie as solicitações recebidas para seus apps com um ALB privado.</td>
</tr>
<tr>
<td><a href="#server-snippets"> Fragmentos do servidor </a></td>
<td><code> server-snippets </code></td>
<td>Inclua uma configuração de bloco do servidor customizado.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotações da conexão</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Anotações da conexão</th>
 <th>Nome</th>
 <th>Descrição</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Tempos limite de conexão e tempos limite de leitura customizados</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Configure o tempo que o ALB aguardará para se conectar e ler o app de backend antes que esse app seja considerado indisponível.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Solicitações de keep-alive</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Configure o número máximo de solicitações que podem ser entregues por meio de uma conexão keep-alive.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Tempo limite de keep-alive</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Configure o tempo máximo que uma conexão keep-alive permanece aberta no servidor.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Próximo envio de dados do proxy</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Configure quando o ALB pode passar uma solicitação para o próximo servidor de envio de dados.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Afinidade de sessão com cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Sempre rotear o tráfego de rede recebido para o mesmo servidor de envio de dados usando um cookie permanente.</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout"> Tempo limite de falha de envio de dados </a></td>
  <td><code> upstream-fail-timeout </code></td>
  <td>Configure a quantia de tempo durante a qual o ALB pode tentar se conectar ao servidor antes que o servidor seja considerado indisponível.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Envio de dados keep-alive</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Configure o número máximo de conexões keep-alive inativas para um servidor de envio de dados.</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails"> Máximo de falhas de envio de dados </a></td>
  <td><code> upstream-max-falha </code></td>
  <td>Configure o número máximo de tentativas malsucedidas para se comunicar com o servidor antes que o servidor seja considerado indisponível.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>Anotações de autenticação HTTPS e TLS/SSL</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>Anotações de autenticação HTTPS e TLS/SSL</th>
  <th>Nome</th>
  <th>Descrição</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">Portas HTTP e HTTPS customizadas</a></td>
  <td><code>custom-port</code></td>
  <td>Mude as portas padrão para o tráfego de rede HTTP (porta 80) e HTTPS (porta 443).</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP redireciona para HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Redirecione solicitações de HTTP não seguras em seu domínio para HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP Strict Transport Security (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>Configure o navegador para acessar o domínio somente usando HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Autenticação mútua</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configure a autenticação mútua para o ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">Suporte a serviços SSL</a></td>
  <td><code>ssl-services</code></td>
  <td>Permita que o suporte de serviços SSL criptografe o tráfego para seus apps de envio de dados que requerem HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">Portas TCP</a></td>
  <td><code>tcp-ports</code></td>
  <td>Acesse um app através de uma porta TCP não padrão.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Anotações de roteamento de caminho</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotações de roteamento de caminho</th>
<th>Nome</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">Serviços externos</a></td>
<td><code>proxy-external-service</code></td>
<td>Inclua definições de caminho para serviços externos, como um serviço hospedado no {{site.data.keyword.Bluemix_notm}}.</td>
</tr>
<tr>
<td><a href="#location-modifier">Modificador de local</a></td>
<td><code>location-modifier</code></td>
<td>Modifique a maneira com que o ALB corresponde o URI de solicitação ao caminho do app.</td>
</tr>
<tr>
<td><a href="#rewrite-path">Gravar novamente caminhos</a></td>
<td><code>rewrite-path</code></td>
<td>Rotear tráfego de rede recebido para um caminho diferente no qual seu app backend atenda.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotações de buffer de proxy</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Anotações de buffer de proxy</th>
 <th>Nome</th>
 <th>Descrição</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">Buffers de cabeçalhos do cliente grandes</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Configure o número e o tamanho máximo dos buffers que leem cabeçalhos de solicitação de cliente grandes.</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">Armazenamento em buffer de dados de resposta do cliente</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Desative o armazenamento em buffer de uma resposta do cliente no ALB ao enviar a resposta para o cliente.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Buffers de proxy</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Configure o número e o tamanho dos buffers que leem uma resposta para uma conexão única do servidor com proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Tamanho do buffer de proxy</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Configure o tamanho do buffer que lê a primeira parte da resposta que é recebida do servidor com proxy.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Tamanho de buffers ocupados de proxy</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Configure o tamanho de buffers de proxy que possam estar ocupados.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Anotações de solicitação e de resposta</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotações de solicitação e de resposta</th>
<th>Nome</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port"> Incluir a porta do servidor no cabeçalho do host </a></td>
<td><code>add-host-port</code></td>
<td>Inclua a porta do servidor para o host para solicitações de roteamento.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Tamanho do corpo da solicitação do cliente</a></td>
<td><code>client-max-body-size</code></td>
<td>Configure o tamanho máximo do corpo que o cliente pode enviar como parte de uma solicitação.</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">Solicitação do cliente ou cabeçalho de resposta adicional</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Inclua informações do cabeçalho em uma solicitação do cliente antes de encaminhar a solicitação para seu app backend ou para uma resposta do cliente antes de enviar a resposta para o cliente.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Remoção do cabeçalho de resposta do cliente</a></td>
<td><code>response-remove-headers</code></td>
<td>Remover informações do cabeçalho de uma resposta do cliente antes de encaminhar a resposta para o cliente.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotações de limite de serviço</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotações de limite de serviço</th>
<th>Nome</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Limites de taxa global</a></td>
<td><code>global-rate-limit</code></td>
<td>Limite a taxa de processamento de solicitação e o número de conexões por uma chave definida para todos os serviços.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Limites de taxa de serviço</a></td>
<td><code>service-rate-limit</code></td>
<td>Limitar a taxa de processamento de solicitação e o número de conexões por uma chave definida para serviços específicos.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Anotações de autenticação do usuário</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Anotações de autenticação do usuário</th>
<th>Nome</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">Autenticação do {{site.data.keyword.appid_short}}</a></td>
<td><code>appid-auth</code></td>
<td>Use o {{site.data.keyword.appid_full}} para autenticar com seu app.</td>
</tr>
</tbody></table>

<br>

## Anotações gerais
{: #general}

### Ações de erro customizadas (`custom-errors`, `custom-error-actions`)
{: #custom-errors}

Indique as ações customizadas que o ALB pode tomar para erros de HTTP específicos.
{: shortdesc}

**Descrição**</br>
Para manipular erros de HTTP específicos que podem ocorrer, é possível configurar ações de erro customizadas a serem tomadas pelo ALB.

* A anotação `custom-errors` define o nome do serviço, o erro HTTP a ser manipulado e o nome da ação de erro que o ALB executa quando encontra o erro de HTTP especificado para o serviço.
* A anotação `custom-error-actions` define ações de erro customizadas em trechos de código NGINX.

Por exemplo, na anotação `custom-errors`, é possível configurar o ALB para manipular erros HTTP `401` para `app1`, retornando uma ação de erro customizada chamada `/errorAction401`. Em seguida, na anotação `custom-error-actions`, é possível definir um fragmento de código chamado `/errorAction401` para que o ALB retorne uma página de erro customizada para o cliente.</br>

Também é possível usar a anotação `custom-errors` para redirecionar o cliente para um serviço de erro que você gerencia. Deve-se definir o caminho para esse serviço de erro na seção `paths` do arquivo de recursos do Ingress.

**YAML de recurso do Ingresso de amostra**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/custom-errors: "serviceName=<app1> httpError=<401> errorActionName=</errorAction401>;serviceName=<app2> httpError=<403> errorActionName=</errorPath>"
    ingress.bluemix.net/custom-error-actions: |
         errorActionName=</errorAction401>
         #Example custom error snippet
         proxy_pass http://example.com/forbidden.html;
         <EOS>
  spec:
    tls:
    - hosts:
      - mydomain
      secretName: mysecret
    rules:
    - host: mydomain
      http:
        paths:
        - path: /path1
          backend:
            serviceName: app1
            servicePort: 80
        - path: /path2
          backend:
            serviceName: app2
            servicePort: 80
        - path: </errorPath>
          backend:
            serviceName: <error-svc>
            servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>app1</em>&gt;</code> pelo nome do serviço do Kubernetes ao qual o erro customizado se aplica. O erro customizado se aplica apenas aos caminhos específicos que usam esse mesmo serviço de envio de dados. Se você não configurar um nome de serviço, os erros customizados serão aplicados a todos os caminhos de serviço.</td>
</tr>
<tr>
<td><code> httpError </code></td>
<td>Substitua <code>&lt;<em>401</em>&gt;</code> pelo código de erro HTTP que você deseja manipular com uma ação de erro customizada.</td>
</tr>
<tr>
<td><code> errorActionName </code></td>
<td>Substitua <code>&lt;<em>/errorAction401</em>&gt;</code> pelo nome de uma ação de erro customizada a ser tomada ou o caminho para um serviço de erro.<ul>
<li>Se você especificar o nome de uma ação de erro customizada, você deverá definir essa ação de erro em um fragmento de código na anotação <code>custom-error-actions</code>. No YAML de amostra, <code>app1</code> usa <code>/errorAction401</code>, que é definido no fragmento na anotação <code>custom-error-actions</code>.</li>
<li>Se você especificar o caminho para um serviço de erro, deverá especificar esse caminho de erro e o nome do serviço de erro na seção <code>paths</code>. No YAML de amostra, <code>app2</code> usa <code>/errorPath</code>, que é definido no final da seção <code>paths</code>.</li></ul></td>
</tr>
<tr>
<td><code> ingresso s.bluemix.net/custom-error-actions </code></td>
<td>Defina uma ação de erro customizada que o ALB executa para o erro de serviço e de HTTP que você especificou. Use um snippet de código NGINX e termine cada fragmento com <code>&lt;EOS&gt;</code>. No YAML de amostra, o ALB passa uma página de erro customizada, <code>http://example.com/forbidden.html</code>, para o cliente quando um erro <code>401</code> ocorre para <code>app1</code>.</td>
</tr>
</tbody></table>

<br />


### Fragmentos de local (` location-snippets `)
{: #location-snippets}

Inclua uma configuração de bloco de local customizado para um serviço.
{:shortdesc}

**Descrição**</br> um bloco de servidor é uma diretiva do NGINX que define a configuração para o servidor virtual do ALB. Um bloco de localização é uma diretiva NGINX definida dentro do bloco do servidor. Os blocos de local definem como o Ingresso processa o URI de solicitação ou a parte da solicitação que vem após o nome de domínio ou endereço IP e porta.

Quando um bloco de servidor recebe uma solicitação, o bloco de local corresponde ao URI para um caminho e a solicitação é encaminhada para o endereço IP do pod no qual o app está implementado. Usando a anotação `location-snippets`, é possível modificar como o bloco de local encaminha as solicitações para serviços específicos.

Para modificar o bloco do servidor como um todo, consulte a anotação [`server-snippets`](#server-snippets).

Para visualizar blocos do servidor e do local no arquivo de configuração NGINX, execute o comando a seguir para um de seus pods do ALB: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML de recurso do Ingresso de amostra**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-snippets: |
      serviceName=<myservice1>
      # Example location snippet
      proxy_request_buffering off;
      rewrite_log on;
      proxy_set_header "x-additional-test-header" "location-snippet-header";
      <EOS>
      serviceName=<myservice2>
      proxy_set_header Authorization "";
      <EOS>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço que você criou para seu app.</td>
</tr>
<tr>
<td>Fragmento de local</td>
<td>Forneça o fragmento de configuração que você deseja usar para o serviço especificado. O fragmento de amostra para o serviço <code>myservice1</code> configura o bloco de local para desativar o armazenamento em buffer de solicitações de proxy, ativar regravações de log e configurar cabeçalhos adicionais quando encaminha uma solicitação para o serviço. O fragmento de amostra para o serviço <code>myservice2</code> configura um cabeçalho <code>Authorization</code> vazio. Cada fragmento de local deve terminar com o valor <code>&lt;EOS&gt;</code>.</td>
</tr>
</tbody></table>

<br />


### Roteamento do ALB privado (` ALB-ID `)
{: #alb-id}

Roteie as solicitações recebidas para seus apps com um ALB privado.
{:shortdesc}

**Descrição**</br>
Em vez do ALB público, escolha um ALB privado para rotear solicitações recebidas.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/ALB-ID: "<private_ALB_ID_1>;<private_ALB_ID_2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>O ID para seu ALB privado. Para localizar o ID de ALB privado, execute <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>.<p>
Se você tiver um cluster de múltiplas zonas com mais de um ALB privado ativado, será possível fornecer uma lista de IDs de ALB separados por <code>;</code>. Por exemplo: <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### Fragmentos do servidor (` server-snippets `)
{: #server-snippets}

Inclua uma configuração de bloco do servidor customizado.
{:shortdesc}

**Descrição**</br> um bloco de servidor é uma diretiva do NGINX que define a configuração para o servidor virtual do ALB. Ao fornecer um fragmento de configuração customizado na anotação `server-snippets`, é possível modificar como o ALB manipula solicitações no nível do servidor.

Para visualizar blocos do servidor e do local no arquivo de configuração NGINX, execute o comando a seguir para um de seus pods do ALB: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**YAML de recurso do Ingresso de amostra**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/server-snippets: |
      # Example snippet
      location = /health {
      return 200 'Healthy';
      add_header Content-Type text/plain;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td>Fragmento do servidor</td>
<td>Forneça o fragmento de configuração que você deseja usar. Esse fragmento de amostra especifica um bloco de local para manipular as solicitações <code>/health</code>. O bloco de local é configurado para retornar uma resposta funcional e incluir um cabeçalho quando ele encaminha uma solicitação.</td>
</tr>
</tbody>
</table>

É possível usar a anotação `server-snippets` para incluir um cabeçalho para todas as respostas de serviço em um nível de servidor:
{: tip}

```
annotations: ingress.bluemix.net/server-snippets: | add_header <header1> <value1>;
```
{: codeblock}

<br />


## Anotações da conexão
{: #connection}

Com as anotações de conexão, é possível mudar como o ALB se conecta ao app de back-end e servidores de envio de dados e configurar tempos limites ou um número máximo de conexões keep-alive antes que o app ou o servidor seja considerado indisponível.
{: shortdesc}

### Tempos limite de conexão e de leitura customizados (`proxy-connect-timeout`, `proxy-read-timeout`)
{: #proxy-connect-timeout}

Configure o tempo que o ALB aguardará para se conectar e ler o app de backend antes que esse app seja considerado indisponível.
{:shortdesc}

**Descrição**</br>
Quando uma solicitação do cliente é enviada ao ALB do Ingress, uma conexão com o aplicativo de back-end é aberta por ele. Por padrão, o ALB aguarda 60 segundos para receber uma resposta do app backend. Se o app backend não responder dentro de 60 segundos, a solicitação de conexão será interrompida e o app backend será considerado indisponível.

Depois que o ALB é conectado ao app backend, os dados de resposta são lidos do app backend pelo ALB. Durante essa operação de leitura, o ALB aguarda um máximo de 60 segundos entre duas operações de leitura para receber dados do app backend. Se o app backend não enviar dados dentro de 60 segundos, a conexão com o app backend será encerrada e o app será considerado não disponível.

Um tempo limite de conexão e de leitura de 60 segundos é o tempo limite padrão em um proxy e geralmente não deve ser mudado.

Se a disponibilidade de seu app não for estável ou seu app estiver lento para responder em razão de altas cargas de trabalho, talvez você queira aumentar o tempo limite de conexão ou de leitura. Lembre-se de que aumentar o tempo limite afeta o desempenho do ALB porque a conexão com o app backend deve permanecer aberta até que o tempo limite seja atingido.

Por outro lado, é possível diminuir o tempo limite para ganhar desempenho no ALB. Assegure-se de que seu app backend seja capaz de manipular solicitações dentro do tempo limite especificado, mesmo durante cargas de trabalho mais altas.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-connect-timeout: "serviceName=<myservice> timeout=<connect_timeout>"
   ingress.bluemix.net/proxy-read-timeout: "serviceName=<myservice> timeout=<read_timeout>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>O número de segundos ou minutos a aguardar para se conectar ao app de backend, por exemplo, <code>65s</code> ou <code>1m</code>. Um tempo limite de conexão não pode exceder 75 segundos.</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>O número de segundos ou minutos a esperar antes de o app de backend ser lido, por exemplo <code>65s</code> ou <code>2m</code>.
</tr>
</tbody></table>

<br />


### Pedidos keep-alive (`keepalive-requests`)
{: #keepalive-requests}

**Descrição**</br>
Configura o número máximo de solicitações que podem ser entregues por meio de uma conexão keep-alive.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/keepalive-requests: "serviceName=<myservice> requests=<max_requests>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: <myservice>
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app. Este parâmetro é opcional. A configuração é aplicada a todos os serviços no subdomínio de Ingress, a menos que um serviço seja especificado. Se o parâmetro for fornecido, as solicitações de keep-alive serão configuradas para o serviço especificado. Se o parâmetro não for fornecido, as solicitações de keep-alive serão configuradas no nível do servidor do <code>nginx.conf</code> para todos os serviços que não tiverem solicitações de keep-alive configuradas.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Substitua <code>&lt;<em>max_requests</em>&gt;</code> pelo número máximo de solicitações que podem ser entregues por meio de uma conexão keep-alive.</td>
</tr>
</tbody></table>

<br />


### Tempo limite de keep-alive (` keepalive-tempo limite `)
{: #keepalive-timeout}

**Descrição**</br>
Configura o tempo máximo de abertura de uma conexão keep-alive no servidor.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/keepalive-timeout: "serviceName=<myservice> timeout=<time>s"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app. Este parâmetro é opcional. Se o parâmetro for fornecido, o tempo limite de keep-alive será configurado para o serviço especificado. Se o parâmetro não for fornecido, o tempo limite de keep-alive será configurado no nível do servidor do <code>nginx.conf</code> para todos os serviços que não têm o tempo limite de keep-alive configurado.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Substitua <code>&lt;<em>time</em>&gt;</code> por uma quantia de tempo em segundos. Exemplo:<code>timeout=20s</code>. Um valor <code>0</code> desativa as conexões do cliente keep-alive.</td>
</tr>
</tbody></table>

<br />


### Proxy próximo envio de dados (` proxy-next-upstream-config `)
{: #proxy-next-upstream-config}

Configure quando o ALB pode passar uma solicitação para o próximo servidor de envio de dados.
{:shortdesc}

**Descrição**</br>
O ALB do Ingress atua como um proxy entre o aplicativo cliente e o seu aplicativo. Algumas configurações de app requerem múltiplos servidores de envio de dados que manipulam solicitações de cliente recebidas do ALB. Às vezes, o servidor proxy usado pelo ALB não pode estabelecer uma conexão com um servidor de envio de dados que o app usa. O ALB poderá então tentar estabelecer uma conexão com o próximo servidor de envio de dados para passar a solicitação para ele, como alternativa. É possível usar a anotação `proxy-next-upstream-config` para configurar em que casos, quanto tempo e quantas vezes o ALB pode tentar passar uma solicitação para o próximo servidor de envio de dados.

O tempo limite é sempre configurado quando você usa `proxy-next-upstream-config`, portanto, não inclua `timeout=true` nessa anotação.
{: note}

**YAML de recurso do Ingresso de amostra**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-next-upstream-config: "serviceName=<myservice1> retries=<tries> timeout=<time> error=true http_502=true; serviceName=<myservice2> http_403=true non_idempotent=true"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Substitua <code>&lt;<em>tries</em>&gt;</code> pela quantia máxima de vezes que o ALB tentará passar uma solicitação para o próximo servidor de envio de dados. Esse número inclui a solicitação original. Para desativar essa limitação, use <code>0</code>. Se você não especificar um valor, o valor padrão <code>0</code> será usado.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Substitua <code>&lt;<em>time</em>&gt;</code> pelo período de tempo máximo, em segundos, que o ALB tentará passar uma solicitação para o próximo servidor de envio de dados. Por exemplo, para configurar um tempo de 30 segundos, insira <code>30s</code>. Para desativar essa limitação, use <code>0</code>. Se você não especificar um valor, o valor padrão <code>0</code> será usado.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>Se configurado como <code>true</code>, o ALB passará uma solicitação para o próximo servidor de envio de dados quando um erro tiver ocorrido ao estabelecer uma conexão com o primeiro servidor de envio de dados, ao passar uma solicitação para ele ou ao ler o cabeçalho de resposta.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>Se configurado como <code>true</code>, o ALB passará uma solicitação para o próximo servidor de envio de dados quando o primeiro servidor de envio de dados retornar uma resposta vazia ou inválida.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>Se configurado como <code>true</code>, o ALB passará uma solicitação para o próximo servidor de envio de dados quando o primeiro servidor de envio de dados retornar uma resposta com o código 502. É possível designar os códigos de resposta HTTP a seguir: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>Se configurado como <code>true</code>, o ALB poderá passar solicitações com um método não idempotente para o próximo servidor de envio de dados. Por padrão, o ALB não passa essas solicitações para o próximo servidor de envio de dados.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>Para evitar que o ALB passe solicitações para o próximo servidor de envio de dados, configure como <code>true</code>.
</td>
</tr>
</tbody></table>

<br />


### Sessão-affinity com cookies (` sticky-cookie-services `)
{: #sticky-cookie-services}

Use a anotação de cookie permanente para incluir a afinidade de sessão para o ALB e sempre rotear o tráfego de rede recebido para o mesmo servidor de envio de dados.
{:shortdesc}

**Descrição**</br>
Para obter alta disponibilidade, algumas configurações de aplicativo requerem que você implemente diversos servidores de envio de dados que manipulem solicitações do cliente recebidas. Quando um cliente se conecta ao app backend, é possível usar a afinidade de sessão para que um cliente seja atendido pelo mesmo servidor de envio de dados pela duração de uma sessão ou pelo tempo que leva para concluir uma tarefa. É possível configurar seu ALB para assegurar a afinidade de sessão sempre roteando o tráfego de rede recebido para o mesmo servidor de envio de dados.

Cada cliente que se conecta ao seu app backend é designado a um dos servidores de envio de dados disponíveis pelo ALB. O ALB cria um cookie de sessão que é armazenado no app do cliente, que está incluído nas informações do cabeçalho de cada solicitação entre o ALB e o cliente. As informações no cookie asseguram que todas as solicitações sejam manipuladas pelo mesmo servidor de envio de dados em toda a sessão.

Confiar em sessões adesivas pode incluir complexidade e reduzir sua disponibilidade. Por exemplo, você pode ter um servidor HTTP que mantém algum estado de sessão para uma conexão inicial para que o serviço HTTP aceite apenas solicitações subsequentes com o mesmo valor de estado de sessão. No entanto, isso evita o fácil ajuste de escala horizontal do serviço HTTP. Considere o uso de um banco de dados externo, como Redis ou Memcached, para armazenar o valor da sessão de solicitação de HTTP para que seja possível manter o estado da sessão entre múltiplos servidores.
{: note}

Ao incluir múltiplos serviços, use um ponto e vírgula (;) para separá-los.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/sticky-cookie-services: "serviceName=<myservice1> name=<cookie_name1> expires=<expiration_time1> path=<cookie_path1> hash=<hash_algorithm1>;serviceName=<myservice2> name=<cookie_name2> expires=<expiration_time2> path=<cookie_path2> hash=<hash_algorithm2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Substitua <code>&lt;<em>cookie_name</em>&gt;</code> pelo nome de um cookie permanente que é criado durante uma sessão.</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>Substitua <code>&lt;<em>expiration_time</em>&gt;</code> pelo tempo em segundos (s), minutos (m) ou horas (h) antes da expiração do cookie permanente. Esse tempo é independente da atividade do usuário. Depois que o cookie expira, ele é excluído pelo navegador da web do cliente e não é mais enviado para o ALB. Por exemplo, para configurar um prazo de expiração de 1 segundo, 1 minuto ou 1 hora, insira <code>1s</code>, <code>1m</code> ou <code>1h</code>.</td>
</tr>
<tr>
<td><code>path</code></td>
<td>Substitua <code>&lt;<em>cookie_path</em>&gt;</code> pelo caminho que é anexado ao subdomínio do Ingresso e que indica para quais domínios e subdomínios o cookie é enviado para o ALB. Por exemplo, se o seu domínio do Ingresso for <code>www.myingress.com</code> e você desejar enviar o cookie em cada solicitação do cliente, deverá configurar <code>path=/</code>. Se desejar enviar o cookie somente para <code>www.myingress.com/myapp</code> e todos os seus subdomínios, então deverá configurar <code>path=/myapp</code>.</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>Substitua <code>&lt;<em>hash_algorithm</em>&gt;</code> pelo algoritmo hash que protege as informações no cookie. Somente <code>sha1
</code> é suportado. SHA1 cria uma soma de hash com base nas informações no cookie e anexa essa soma de hash ao cookie. O servidor pode decriptografar as informações no cookie e verificar a integridade de dados.</td>
</tr>
</tbody></table>

<br />


### Tempo limite de failover de envio de dados (` upstream-fail-timeout `)
{: #upstream-fail-timeout}

Configure a quantia de tempo durante a qual o ALB pode tentar se conectar ao servidor.
{:shortdesc}

**Descrição**</br>
Configure a quantidade de tempo durante a qual o ALB pode tentar se conectar a um servidor antes que o servidor seja considerado indisponível. Para que um servidor seja considerado indisponível, o ALB deve atingir o número máximo de tentativas de conexão com falha configurado pela anotação [`upstream-max-fails`](#upstream-max-fails) dentro da quantidade de tempo configurada. Essa quantia de tempo também determina por quanto tempo o servidor é considerado indisponível.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-fail-timeout: "serviceName=<myservice> fail-timeout=<fail_timeout>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>Substitua <code>&lt;<em>fail_timeout</em>&gt;</code> pela quantia de tempo que o ALB pode tentar se conectar a um servidor antes que o servidor seja considerado indisponível. O padrão é  <code> 10s </code>. O tempo deve ser em segundos.</td>
</tr>
</tbody></table>

<br />


### Upstream keepalive (` upstream-keepalive `)
{: #upstream-keepalive}

Configure o número máximo de conexões keep-alive inativas para um servidor de envio de dados.
{:shortdesc}

**Descrição**</br>
Configure o número máximo de conexões keep-alive inativas com o servidor de envio de dados de um determinado serviço. O servidor de envio de dados tem 64 conexões keep-alive inativas por padrão.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-keepalive: "serviceName=<myservice> keepalive=<max_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>Substitua <code>&lt;<em>max_connections</em>&gt;</code> pelo número máximo de conexões keep-alive inativas para o servidor de envio de dados. O padrão é <code>64</code>. Um valor <code>0</code> desativa conexões keep-alive de envio de dados para o serviço especificado.</td>
</tr>
</tbody></table>

<br />


### O máximo de fluxo de atualização falha (` upstream-max-falha `)
{: #upstream-max-fails}

Configure o número máximo de tentativas malsucedidas para se comunicar com o servidor.
{:shortdesc}

**Descrição**</br>
Configure o número máximo de vezes que o ALB pode falhar ao se conectar com o servidor antes que o servidor seja considerado indisponível. Para que o servidor seja considerado indisponível, o ALB deve atingir o número máximo dentro da duração de tempo configurada pela anotação [`upstream-fail-timeout`](#upstream-fail-timeout). A duração de tempo em que o servidor é considerado indisponível também é configurada pela anotação `upstream-fail-timeout`.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/upstream-max-fails: "serviceName=<myservice> max-fails=<max_fails>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code> max-falha </code></td>
<td>Substitua <code>&lt;<em>max_fails</em>&gt;</code> pelo número máximo de tentativas malsucedidas que o ALB pode fazer para se comunicar com o servidor. O padrão é <code>1</code>. Um valor <code>0</code> desativa a anotação.</td>
</tr>
</tbody></table>

<br />


## Anotações de autenticação HTTPS e TLS/SSL
{: #https-auth}

Com as anotações de autenticação HTTPS e TLS/SSL, é possível configurar seu ALB para tráfego HTTPS, mudar as portas HTTPS padrão, ativar a criptografia SSL para o tráfego que é enviado para seus apps de back-end ou configurar a autenticação mútua.
{: shortdesc}

### Portas HTTP e HTTPS customizadas (`custom-port`)
{: #custom-port}

Mude as portas padrão para o tráfego de rede HTTP (porta 80) e HTTPS (porta 443).
{:shortdesc}

**Descrição**</br>
Por padrão, o ALB do Ingress é configurado para atender o tráfego de rede HTTP recebido na porta 80 e o tráfego de rede HTTPS recebido na porta 443. É possível mudar as portas padrão para incluir segurança em seu domínio do ALB ou para ativar somente uma porta HTTPS.

Para ativar a autenticação mútua em uma porta, [configure o ALB para abrir a porta válida](/docs/containers?topic=containers-ingress#opening_ingress_ports) e, em seguida, especifique essa porta na anotação [`mutual-auth`](#mutual-auth). Não use a anotação `custom-port` para especificar uma porta para autenticação mútua.
{: note}

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/custom-port: "protocol=<protocol1> port=<port1>;protocol=<protocol2> port=<port2>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Insira <code>http</code> ou <code>https</code> para mudar a porta padrão para o tráfego de rede recebido HTTP ou HTTPS.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Insira o número da porta a ser usado para o tráfego de rede recebido HTTP ou HTTPS.  <p class="note">Quando uma porta customizada é especificada para HTTP ou HTTPS, as portas padrão não são mais válidas para HTTP e HTTPS. Por exemplo, para mudar a porta padrão para HTTPS para 8443, mas usar a porta padrão para HTTP, deve-se configurar portas customizadas para ambos: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
</tbody></table>


**Uso**</br>
1. Revise as portas abertas de seu ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  A saída da CLI é semelhante à seguinte:
  ```
  NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE public-cr18e61e63c6e94b658596ca93d087eed9-alb1 LoadBalancer 10.xxx.xx.xxx 169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP 109d
  ```
  {: screen}

2. Abra o mapa de configuração do ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Inclua as portas HTTP e HTTPS não padrão no mapa de configuração. Substitua `<port>` pela porta HTTP ou HTTPS que deseja abrir.
  <p class="note">Por padrão, as portas 80 e 443 ficam abertas. Se deseja manter a 80 e a 443 abertas, deve-se também incluí-las além de quaisquer outras portas TCP especificadas no campo `public-ports`. Quando se ativa um ALB privado, deve-se também especificar quaisquer portas que deseja manter abertas no campo `private-ports`. Para obter mais informações, veja [Abrindo portas no ALB do Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).</p>
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: <port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
  ```
  {: codeblock}

4. Verifique se o ALB está reconfigurado com as portas não padrão.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  A saída da CLI é semelhante à seguinte:
  ```
  NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE public-cr18e61e63c6e94b658596ca93d087eed9-alb1 LoadBalancer 10.xxx.xx.xxx 169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP 109d
  ```
  {: screen}

5. Configure seu Ingresso para usar as portas não padrão ao rotear o tráfego de rede recebido para seus serviços. Use a anotação no arquivo YAML de amostra nessa referência.

6. Atualize sua configuração do ALB.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Abra seu navegador da web preferencial para acessar seu app. Exemplo: `https://<ibmdomain>:<port>/<service_path>/`

<br />


### Redirecionamentos de HTTP para HTTPS (`redirect-to-https`)
{: #redirect-to-https}

Converta as solicitações não seguras do cliente HTTP para HTTPS.
{:shortdesc}

**Descrição**</br>
Configure seu ALB do Ingress para proteger seu domínio com o certificado TLS fornecido pela IBM ou com seu certificado TLS customizado. Alguns usuários podem tentar acessar seus apps usando uma solicitação de `http` não segura para seu domínio do ALB, por exemplo `http://www.myingress.com`, em vez de usar `https`. É possível usar a anotação de redirecionamento para sempre converter solicitações de HTTP não seguras para HTTPS. Se você não usar essa anotação, as solicitações de HTTP não seguras não serão convertidas em solicitações de HTTPS por padrão e poderão expor informações confidenciais não criptografadas ao público.


O redirecionamento de solicitações de HTTP para HTTPS é desativado por padrão.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/redirect-to-https: "True"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<br />


### HTTP Strict Transport Security (`hsts`)
{: #hsts}

**Descrição**</br>
O HSTS instrui o navegador a acessar um domínio apenas por meio de HTTPS. Mesmo se o usuário insere ou segue um link HTTP simples, o navegador faz upgrade estritamente da conexão com HTTPS.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/hsts: enabled=true maxAge=<31536000> includeSubdomains=true
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>ativado</code></td>
<td>Use <code>true</code> para ativar o HSTS.</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>Substitua <code>&lt;<em>31536000</em>&gt;</code> por um número inteiro representando quantos segundos um navegador armazenará em cache as solicitações de envio diretamente para HTTPS. O padrão é <code>31536000</code>, que é igual a 1 ano.</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>Use <code>true</code> para informar ao navegador que a política HSTS também se aplica a todos os subdomínios do domínio atual. O padrão é <code>true</code>. </td>
</tr>
</tbody></table>

<br />


### Autenticação mútua (` mutual-auth `)
{: #mutual-auth}

Configure a autenticação mútua para o ALB.
{:shortdesc}

**Descrição**</br>
Configure a autenticação mútua do tráfego de recebimento de dados para o ALB do Ingress. O cliente externo autentica o servidor e o servidor também autentica o cliente usando certificados. A autenticação mútua também é conhecida como autenticação baseada em certificado ou autenticação de duas vias.

Use a anotação `mutual-auth` para a finalização de SSL entre o cliente e o ALB do Ingress. Use a [anotação `ssl-services`](#ssl-services) para a finalização de SSL entre o ALB do Ingress e o app de back-end.

A anotação de autenticação mútua valida os certificados de cliente. Para encaminhar os certificados de cliente em um cabeçalho para os aplicativos para manipular a autorização, é possível usar a anotação [`proxy-add-headers` a seguir](#proxy-add-headers): `"ingresss.bluemix.net/proxy-add-headers ":" serviceName = router-set { \n X-Forwarded-Client-Cert $ssl_client_escaped_cert; \n} \n "`
{: tip}

**Pré-requisitos**</br>

* Deve-se ter um segredo de autenticação mútua válido que contenha o `ca.crt` necessário. Para criar um segredo de autenticação mútua, consulte as etapas no final desta seção.
* Para ativar a autenticação mútua em uma porta diferente de 443, [configure o ALB para abrir a porta válida](/docs/containers?topic=containers-ingress#opening_ingress_ports) e, em seguida, especifique essa porta nesta anotação. Não use a anotação `custom-port` para especificar uma porta para autenticação mútua.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/mutual-auth: "secretName=<mysecret> port=<port> serviceName=<servicename1>,<servicename2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Substitua <code>&lt;<em>mysecret</em>&gt;</code> por um nome para o recurso secreto.</td>
</tr>
<tr>
<td><code>port</code></td>
<td>Substitua <code>&lt;<em>port</em>&gt;</code> pelo número da porta ALB.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>servicename</em>&gt;</code> pelo nome de um ou mais recursos do Ingress. Este parâmetro é opcional.</td>
</tr>
</tbody></table>

**Para criar um segredo de autenticação mútua:**

1. Gere um certificado de autoridade de certificação (CA) e a chave por meio do provedor de certificado. Se você tiver seu próprio domínio, compre um certificado TLS oficial para seu domínio. Certifique-se de que o [CN ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://support.dnsimple.com/articles/what-is-common-name/) seja diferente para cada certificado.
    Para propósitos de teste, é possível criar um certificado auto-assinado usando o OpenSSL. Para obter mais informações, consulte este [tutorial de certificado SSL autoassinado ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.akadia.com/services/ssh_test_certificate.html) ou este [tutorial de autenticação mútua que inclui a criação de sua própria CA ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Converta o certificado na base 64 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.base64encode.org/).
3. Crie um arquivo do YAML secreto usando o cert.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
   ```
   {: codeblock}
4. Crie o certificado como um segredo do Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Suporte a serviços SSL (` ssl-services `)
{: #ssl-services}

Permitir solicitações de HTTPS e criptografar o tráfego para seus apps de envio de dados.
{:shortdesc}

**Descrição**</br>
Quando a configuração de seu recurso do Ingress tiver uma seção TLS, o ALB do Ingress poderá manipular solicitações de URL protegidas por HTTPS para seu aplicativo. Por padrão, o ALB realiza a finalização do TLS e decriptografa a solicitação antes de usar o protocolo HTTP para encaminhar o tráfego para seus apps. Se você tiver apps que requerem o protocolo HTTPS e precisar de tráfego para ser criptografado, use a anotação `ssl-services`. Com a anotação `ssl-services`, o ALB finaliza a conexão TLS externa, em seguida, cria uma nova conexão SSL entre o ALB e o pod de app. O tráfego é criptografado novamente antes de ser enviado para os pods de envio de dados.

Se o seu app de back-end puder manipular o TLS e você desejar incluir segurança adicional, será possível incluir a autenticação unidirecional ou mútua fornecendo um certificado que está contido em um segredo.

Use a anotação `ssl-services` para a finalização de SSL entre o ALB do Ingress e o app de back-end. Use a [anotação `mutual-auth`](#mutual-auth) para a finalização de SSL entre o cliente e o ALB do Ingress.
{: tip}

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: <myingressname>
  annotations:
    ingress.bluemix.net/ssl-services: ssl-service=<myservice1> ssl-secret=<service1-ssl-secret>;ssl-service=<myservice2> ssl-secret=<service2-ssl-secret>
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: myservice1
          servicePort: 8443
      - path: /service2_path
        backend:
          serviceName: myservice2
          servicePort: 8444
          ```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço que requer HTTPS. O tráfego é criptografado do ALB para o serviço desse app.</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>Se o seu app de back-end puder manipular o TLS e você quiser incluir segurança adicional, substitua <code>&lt;<em>service-ssl-secret</em>&gt;</code> pelo segredo de autenticação unidirecional ou mútua do serviço.<ul><li>Se você fornecer um segredo de autenticação unilateral, o valor deverá conter o <code>trusted.crt</code> do servidor de envio de dados. Para criar um segredo unidirecional, veja as etapas no término desta seção.</li><li>Se você fornecer um segredo de autenticação mútua, o valor deverá conter o <code>client.crt</code> e o <code>client.key</code> necessários que seu app está esperando do cliente. Para criar um segredo de autenticação mútua, consulte as etapas no final desta seção.</li></ul><p class="important">Se você não fornecer um segredo, as conexões não seguras serão permitidas. Você pode escolher omitir um segredo se desejar testar a conexão e não tiver certificados prontos ou se seus certificados estiverem expirados e você desejar permitir conexões não seguras.</p></td>
</tr>
</tbody></table>


** Para criar um segredo de autenticação unidirecional: **

1. Obtenha a chave de autoridade de certificação (CA) e o certificado de seu servidor de envio de dados e um certificado de cliente SSL. O IBM ALB é baseado em NGINX, que requer o certificado raiz, o certificado intermediário e o certificado de backend. Para obter mais informações, consulte [Docs do NGINX ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/).
2. [Converta o certificado na base 64 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.base64encode.org/).
3. Crie um arquivo do YAML secreto usando o cert.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       trusted.crt: <ca_certificate>
   ```
   {: codeblock}

   Para também cumprir a autenticação mútua para o tráfego de envio de dados, é possível fornecer um `client.crt` e `client.key`, além do `trusted.crt` na seção de dados.
   {: tip}

4. Crie o certificado como um segredo do Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**Para criar um segredo de autenticação mútua:**

1. Gere um certificado de autoridade de certificação (CA) e a chave por meio do provedor de certificado. Se você tiver seu próprio domínio, compre um certificado TLS oficial para seu domínio. Certifique-se de que o [CN ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://support.dnsimple.com/articles/what-is-common-name/) seja diferente para cada certificado.
    Para propósitos de teste, é possível criar um certificado auto-assinado usando o OpenSSL. Para obter mais informações, consulte este [tutorial de certificado SSL autoassinado ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.akadia.com/services/ssh_test_certificate.html) ou este [tutorial de autenticação mútua que inclui a criação de sua própria CA ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Converta o certificado na base 64 ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.base64encode.org/).
3. Crie um arquivo do YAML secreto usando o cert.
   ```
   apiVersion: v1
     kind: Secret
     metadata:
       name: ssl-my-test
     type: Opaque
     data:
       ca.crt: <ca_certificate>
   ```
   {: codeblock}
4. Crie o certificado como um segredo do Kubernetes.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### Portas TCP (` tcp-ports `)
{: #tcp-ports}

Acesse um app através de uma porta TCP não padrão.
{:shortdesc}

**Descrição**</br>
Use essa anotação para um aplicativo que esteja executando uma carga de trabalho de fluxos TCP.

<p class="note">O ALB opera no modo de passagem e encaminha o tráfego para apps de back-end. A finalização de SSL não é suportada neste caso. A conexão TLS não é finalizada e passa intacta.</p>

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/tcp-ports: "serviceName=<myservice> ingressPort=<ingress_port> servicePort=<service_port>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço do Kubernetes para acessar por meio da porta TCP não padrão.</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>Substitua <code>&lt;<em>ingress_port</em>&gt;</code> pela porta TCP na qual você deseja acessar seu app.</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>Este parâmetro é opcional. Quando fornecido, a porta é substituída para esse valor antes que o tráfego seja enviado para o app de back-end. Caso contrário, a porta permanece a mesma que a porta do Ingress. Se você não quiser configurar esse parâmetro, será possível removê-lo de sua configuração. </td>
</tr>
</tbody></table>


**Uso**</br>
1. Revise as portas abertas de seu ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  A saída da CLI é semelhante à seguinte:
  ```
  NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE public-cr18e61e63c6e94b658596ca93d087eed9-alb1 LoadBalancer 10.xxx.xx.xxx 169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP 109d
  ```
  {: screen}

2. Abra o mapa de configuração do ALB.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Inclua as portas TCP no mapa de configuração. Substitua `<port>` pelas portas TCP que deseja abrir.
  Por padrão, as portas 80 e 443 ficam abertas. Se deseja manter a 80 e a 443 abertas, deve-se também incluí-las além de quaisquer outras portas TCP especificadas no campo `public-ports`. Quando se ativa um ALB privado, deve-se também especificar quaisquer portas que deseja manter abertas no campo `private-ports`. Para obter mais informações, veja [Abrindo portas no ALB do Ingress](/docs/containers?topic=containers-ingress#opening_ingress_ports).
  {: note}
  ```
  apiVersion: v1
  kind: ConfigMap
  data:
    public-ports: 80;443;<port1>;<port2>
  metadata:
    creationTimestamp: 2017-08-22T19:06:51Z
    name: ibm-cloud-provider-ingress-cm
    namespace: kube-system
    resourceVersion: "1320"
    selfLink: /api/v1/namespaces/kube-system/configmaps/ibm-cloud-provider-ingress-cm
    uid: <uid>
   ```
  {: codeblock}

4. Verifique se o ALB está reconfigurado com as portas TCP.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  A saída da CLI é semelhante à seguinte:
  ```
  NAME TYPE CLUSTER-IP EXTERNAL-IP PORT(S) AGE public-cr18e61e63c6e94b658596ca93d087eed9-alb1 LoadBalancer 10.xxx.xx.xxx 169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP 109d
  ```
  {: screen}

5. Configure o ALB para acessar seu aplicativo por meio de uma porta TCP não padrão. Use a anotação `tcp-ports` no arquivo YAML de amostra nessa referência.

6. Crie seu recurso ALB ou atualize a configuração existente do ALB.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Use curl no subdomínio do Ingress para acessar seu app. Exemplo: `curl <domain>:<ingressPort>`

<br />


## Anotações de roteamento de caminho
{: #path-routing}

O ALB do Ingress roteia o tráfego para os caminhos nos quais os aplicativos back-end atendem. Com as anotações de roteamento de caminho, é possível configurar como o ALB roteia o tráfego para seus apps.
{: shortdesc}

### Serviços externos (` proxy-external-service `)
{: #proxy-external-service}

Inclua definições de caminho para serviços externos, como serviços hospedados no {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

**Descrição**</br>
Inclua definições de caminho em serviços externos. Use essa anotação somente quando seu app operar em um serviço externo em vez de um serviço de back-end. Quando você usa essa anotação para criar uma rota de serviço externo, somente as anotações `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout` e `proxy-buffer` são suportadas em conjunto. Quaisquer outras anotações não são suportadas em conjunto com `proxy-external-service`.

Não é possível especificar múltiplos hosts para um único serviço e caminho.
{: note}

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-external-service: "path=<mypath> external-svc=https:<external_service> host=<mydomain>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>Substitua <code>&lt;<em>mypath</em>&gt;</code> pelo caminho em que o serviço externo atende.</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>Substitua <code>&lt;<em>external_service</em>&gt;</code> pelo serviço externo a ser chamado. Por exemplo, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
</tr>
<tr>
<td><code>host</code></td>
<td>Substitua <code>&lt;<em>mydomain</em>&gt;</code> pelo domínio do host para o serviço externo.</td>
</tr>
</tbody></table>

<br />


### Modificador de local (` location-modifier `)
{: #location-modifier}

Modifique a maneira com que o ALB corresponde o URI de solicitação ao caminho do app.
{:shortdesc}

**Descrição**</br>
Por padrão, os ALBs processam os caminhos nos quais os aplicativos atendem como prefixos. Quando um ALB recebe uma solicitação para um app, o ALB verifica o recurso de Ingresso para um caminho (como um prefixo) que corresponde ao início do URI de solicitação. Se uma correspondência é localizada, a solicitação é encaminhada para o endereço IP do pod no qual o app está implementado.

A anotação `location-modifier` muda a maneira como o ALB procura correspondências modificando a configuração de bloco de local. O bloco de local determina como as solicitações são manipuladas para o caminho do app.

Para manipular caminhos de expressão regular (regex), essa anotação é necessária.
{: note}

**Modificadores suportados**</br>
<table>
<caption>Modificadores suportados</caption>
<col width="10%">
<col width="90%">
<thead>
<th>Modificador</th>
<th>Descrição</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>O modificador sinal de igual faz com que o ALB selecione somente as correspondências exatas. Quando uma correspondência exata é localizada, a procura para e o caminho correspondente é selecionado.<br>Por exemplo, se seu app atende em <code>/tea</code>, o ALB seleciona somente caminhos <code>/tea</code> exatos ao corresponder uma solicitação com seu app.</td>
</tr>
<tr>
<td><code>~</code></td>
<td>O modificador til faz com que o ALB processe caminhos como caminhos regex com distinção entre maiúsculas e minúsculas durante a correspondência.<br>Por exemplo, se seu app atende em <code>/coffee</code>, o ALB pode selecionar os caminhos <code>/ab/coffee</code> ou <code>/123/coffee</code> ao corresponder uma solicitação com seu app mesmo que os caminhos não estejam configurados explicitamente para seu app.</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>O til seguido por um modificador asterisco faz com que o ALB processe caminhos como caminhos regex sem distinção entre maiúsculas e minúsculas durante a correspondência.<br>Por exemplo, se seu app atende em <code>/coffee</code>, o ALB pode selecionar os caminhos <code>/ab/Coffee</code> ou <code>/123/COFFEE</code> ao corresponder uma solicitação com seu app mesmo que os caminhos não estejam configurados explicitamente para seu app.</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>O acento circunflexo seguido por um modificador til faz com que o ALB selecione a melhor correspondência não regex em vez de um caminho regex.</td>
</tr>
</tbody>
</table>


**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/location-modifier: "modifier='<location_modifier>' serviceName=<myservice1>;modifier='<location_modifier>' serviceName=<myservice2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>Substitua <code>&lt;<em>location_modifier</em>&gt;</code> pelo modificador de local que você deseja usar para o caminho. Os modificadores suportados são <code>'='</code>, <code>'~'</code>, <code>'~\*'</code> e <code>'^~'</code>. Deve-se colocar os modificadores entre aspas simples.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
</tbody></table>

<br />


### Regravar caminhos (` rewrite-path `)
{: #rewrite-path}

Roteie o tráfego de rede recebido em um caminho de domínio ALB para um caminho diferente no qual seu app back-end atende.
{:shortdesc}

**Descrição**</br>
Seu domínio do ALB do Ingress roteia o tráfego de rede recebido em `mykubecluster.us-south.containers.appdomain.cloud/beans` para seu aplicativo. O seu app atende em `/coffee`, em vez de `/beans`. Para encaminhar o tráfego de rede recebido para o seu app, inclua a anotação de regravação em seu arquivo de configuração do recurso de Ingresso. A anotação de nova gravação assegura que o tráfego de rede recebido em `/beans` seja encaminhado para o seu app usando o caminho `/coffee`. Ao incluir múltiplos serviços, use somente um ponto e vírgula (;) sem espaço antes ou depois do ponto e vírgula para separá-los.

**YAML de recurso do Ingresso de amostra**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/rewrite-path: "serviceName=<myservice1> rewrite=<target_path1>;serviceName=<myservice2> rewrite=<target_path2>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mysecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /beans
        backend:
          serviceName: myservice1
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Substitua <code>&lt;<em>target_path</em>&gt;</code> pelo caminho em que seu app atende. O tráfego de rede recebido no domínio do ALB é encaminhado para o serviço Kubernetes usando esse caminho. A maioria dos apps não atende em um caminho específico, mas usa o caminho raiz e uma porta específica. No exemplo para <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code>, o caminho de regravação é <code>/coffee</code>. <p class= "note">Ao aplicar esse arquivo e receber da URL uma resposta <code>404</code>, será possível que seu aplicativo de back-end esteja atendendo em um caminho que termine em `/`. Tente incluir um `/` final nesse campo de regravação e, em seguida, reaplique o arquivo e tente a URL novamente.</p></td>
</tr>
</tbody></table>

<br />


## Anotações de buffer de proxy
{: #proxy-buffer}

O ALB do Ingress age como um proxy entre seu app backend e o navegador da web do cliente. Com as anotações do buffer de proxy, é possível configurar como os dados são armazenados em buffer em seu ALB ao enviar ou receber pacotes de dados.  
{: shortdesc}

### Buffers grandes de cabeçalho do cliente (`large-client-header-buffers`)
{: #large-client-header-buffers}

Configure o número e o tamanho máximo dos buffers que leem cabeçalhos de solicitação de cliente grandes.
{:shortdesc}

**Descrição**</br>
Os buffers que leem cabeçalhos de solicitação do cliente grandes são alocados somente sob demanda: se uma conexão for transicionada para o estado keep-alive após o processamento de término da solicitação, eles serão liberados. Por padrão, os buffers e o tamanho do buffer `4` são iguais aos bytes `8K`. Se uma linha de solicitação exceder o tamanho máximo configurado de um buffer, o erro de HTTP `414 Request-URI Too Large` será retornado para o cliente. Além disso, se um campo de cabeçalho da solicitação excede o tamanho máximo configurado de um buffer, o erro `400 Bad Request` é retornado para o cliente. É possível ajustar o número máximo e o tamanho dos buffers que são usados para ler cabeçalhos da solicitação do cliente grandes.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/large-client-header-buffers: "number=<number> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>O número máximo de buffers que devem ser alocados para ler o cabeçalho da solicitação do cliente grande. Por exemplo, para configurá-lo como 4, defina <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>O tamanho máximo de buffers que leem o cabeçalho da solicitação do cliente grande. Por exemplo, para configurá-lo como 16 kilobytes, defina <code>16k</code>. O tamanho deve terminar com um <code>k</code> para kilobyte ou <code>m</code> para megabyte.</td>
 </tr>
</tbody></table>

<br />


### Buffer de dados de resposta do cliente (`proxy-buffering`)
{: #proxy-buffering}

Use a anotação de buffer para desativar o armazenamento de dados de resposta no ALB enquanto os dados são enviados para o cliente.
{:shortdesc}

**Descrição**</br> o Ingress ALB age como um proxy entre o seu app de back-end e o navegador da web do cliente. Quando uma resposta é enviada do app backend para o cliente, os dados de resposta são armazenados em buffer no ALB por padrão. O ALB usa o proxy em uma resposta do cliente e começa a enviar a resposta para o cliente no ritmo do cliente. Depois que todos os dados do app backend são recebidos pelo ALB, a conexão com o app backend é encerrada. A conexão do ALB para o cliente permanece aberta até que o cliente receba todos os dados.

Se o armazenamento em buffer de dados de resposta no ALB está desativado, os dados são enviados imediatamente do ALB para o cliente. O cliente deve ser capaz de manipular os dados recebidos no ritmo do ALB. Se o cliente for muito lento, a conexão de envio de dados permanecerá aberta até que o cliente possa alcançá-lo.

O armazenamento em buffer de dados de resposta no ALB é ativado por padrão.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=<false> serviceName=<myservice1>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>ativado</code></td>
<td>Para desativar o armazenamento em buffer de dados de resposta no ALB, configure para <code>false</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code><em>&lt;myservice1&gt;</em></code> pelo nome do serviço do Kubernetes que você criou para o seu app. Separe múltiplos serviços com um ponto e vírgula (;). Este campo é opcional. Se você não especificar um nome do serviço, todos os serviços usarão essa anotação.</td>
</tr>
</tbody></table>

<br />


### Buffers de proxy (` proxy-buffers `)
{: #proxy-buffers}

Configure o número e o tamanho dos buffers de proxy para o ALB.
{:shortdesc}

**Descrição**</br>
Configure o número e o tamanho dos buffers que leem uma resposta para uma conexão única do servidor com proxy. A configuração é aplicada a todos os serviços no subdomínio de Ingress, a menos que um serviço seja especificado. Por exemplo, se uma configuração como `serviceName=SERVICE=2 size=1k` for especificada, 1K será aplicado ao serviço. Se uma configuração, como `number=2 size=1k`, for especificada, 1k será aplicado a todos os serviços no subdomínio do Ingress.</br>
<p class="tip">Se você receber a mensagem de erro `upstream sent too big header while reading response header from upstream`, o servidor de envio de dados em seu back-end enviou um tamanho de cabeçalho que é maior que o limite padrão. Aumente o tamanho para ambos os `proxy-buffers` e [`proxy-buffer-size`](#proxy-buffer-size).</p>

**YAML de recurso do Ingresso de amostra**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffers: "serviceName=<myservice> number=<number_of_buffers> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome de um serviço para aplicar proxy-buffers.</td>
</tr>
<tr>
<td><code>number</code></td>
<td>Substitua <code>&lt;<em>number_of_buffers</em>&gt;</code> por um número, como <code>2</code>.</td>
</tr>
<tr>
<td><code>Padrão</code></td>
<td>Substitua <code>&lt;<em>size</em>&gt;</code> pelo tamanho de cada buffer em kilobytes (k ou K), tal como <code>1K</code>.</td>
</tr>
</tbody>
</table>

<br />


### Tamanho do buffer do proxy (` proxy-buffer-size `)
{: #proxy-buffer-size}

Configure o tamanho do buffer de proxy que lê a primeira parte da resposta.
{:shortdesc}

**Descrição**</br>
Configure o tamanho do buffer que lê a primeira parte da resposta recebida do servidor com proxy. Essa parte da resposta geralmente contém um cabeçalho de resposta pequeno. A configuração é aplicada a todos os serviços no subdomínio de Ingress, a menos que um serviço seja especificado. Por exemplo, se uma configuração como `serviceName=SERVICE size=1k` for especificada, 1k será aplicado ao serviço. Se uma configuração como `size=1k` for especificada, 1k será aplicado a todos os serviços no subdomínio de Ingress.

Se você receber a mensagem de erro `upstream sent too big header while reading response header from upstream`, o servidor de envio de dados em seu back-end enviou um tamanho de cabeçalho que é maior que o limite padrão. Aumente o tamanho para ambos, `proxy-buffer-size` e [`proxy-buffers`](#proxy-buffers).
{: tip}

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-buffer-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome de um serviço para aplicar proxy-buffers-size.</td>
</tr>
<tr>
<td><code>Padrão</code></td>
<td>Substitua <code>&lt;<em>size</em>&gt;</code> pelo tamanho de cada buffer em kilobytes (k ou K), tal como <code>1K</code>. Para calcular o tamanho adequado, é possível consultar [esta postagem do blog ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx). </td>
</tr>
</tbody></table>

<br />


### Tamanho de buffers ocupados do proxy (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

Configure o tamanho de buffers de proxy que podem estar ocupados.
{:shortdesc}

**Descrição**</br>
Limite o tamanho de quaisquer buffers que estejam enviando uma resposta ao cliente enquanto ela ainda não foi totalmente lida. Enquanto isso, o restante dos buffers pode ler a resposta e, se necessário, parte do buffer da resposta em um arquivo temporário. A configuração é aplicada a todos os serviços no subdomínio de Ingress, a menos que um serviço seja especificado. Por exemplo, se uma configuração como `serviceName=SERVICE size=1k` for especificada, 1k será aplicado ao serviço. Se uma configuração como `size=1k` for especificada, 1k será aplicado a todos os serviços no subdomínio de Ingress.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: proxy-ingress
 annotations:
   ingress.bluemix.net/proxy-busy-buffers-size: "serviceName=<myservice> size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
         ```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome de um serviço para aplicar proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>Padrão</code></td>
<td>Substitua <code>&lt;<em>size</em>&gt;</code> pelo tamanho de cada buffer em kilobytes (k ou K), tal como <code>1K</code>.</td>
</tr>
</tbody></table>

<br />


## Anotações de solicitação e de resposta
{: #request-response}

Use as anotações de solicitação e de resposta para incluir ou remover informações do cabeçalho das solicitações do cliente e do servidor e para mudar o tamanho do corpo que o cliente pode enviar.
{: shortdesc}

### Inclua a porta do servidor no cabeçalho do host (`add-host-port`)
{: #add-host-port}

Inclua uma porta do servidor na solicitação do cliente antes que a solicitação seja encaminhada para seu app de back-end.
{: shortdesc}

**Descrição**</br>
Inclua o `:server_port` no cabeçalho do host de uma solicitação do cliente antes de encaminhá-la ao seu aplicativo de back-end.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/add-host-port: "enabled=<true> serviceName=<myservice>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>ativado</code></td>
<td>Para ativar a configuração de server_port para o subdomínio, configure como <code>true</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code><em>&lt;myservice&gt;</em></code> pelo nome do serviço Kubernetes que você criou para o seu app. Separe múltiplos serviços com um ponto e vírgula (;). Este campo é opcional. Se você não especificar um nome do serviço, todos os serviços usarão essa anotação.</td>
</tr>
</tbody></table>

<br />


### Solicitação do cliente ou cabeçalho de resposta adicional (`proxy-add-headers`, `response-add-headers`)
{: #proxy-add-headers}

Inclua informações extras do cabeçalho em uma solicitação do cliente antes de enviar a solicitação para o app backend ou para uma resposta do cliente antes de enviar a resposta ao cliente.
{:shortdesc}

**Descrição**</br>
O ALB do Ingress atua como um proxy entre o aplicativo cliente e seu aplicativo de back-end. As solicitações do cliente que são enviadas para o ALB são processadas (por proxy) e colocadas em uma nova solicitação que é então enviada para seu app backend. Da mesma forma, as respostas de app de back-end que são enviadas para o ALB são processadas (proxy) e colocadas em uma nova resposta que é enviada para o cliente. O uso de proxy em uma solicitação ou resposta remove informações de cabeçalho HTTP, como o nome do usuário, que foram enviadas inicialmente do cliente ou app backend.

Se o seu app backend requer informações do cabeçalho de HTTP, é possível usar a anotação `proxy-add-headers` para incluir informações do cabeçalho na solicitação do cliente antes que a solicitação seja encaminhada pelo ALB para o app backend. Se o app da web do cliente requer informações do cabeçalho de HTTP, é possível usar a anotação `response-add-headers` para incluir informações do cabeçalho na resposta antes que a resposta seja encaminhada pelo ALB para o app da web do cliente.<br>

Por exemplo, talvez você precise incluir as informações do cabeçalho X-Forward a seguir na solicitação antes que ela seja encaminhada para o seu app:
```
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
```
{: screen}

Para incluir as informações do cabeçalho X-Forward na solicitação que é enviada para seu app, use a anotação `proxy-add-headers` da maneira a seguir:
```
ingress.bluemix.net/proxy-add-headers: |
  serviceName=<myservice1> {
  Host $host;
  X-Real-IP $remote_addr;
  X-Forwarded-Proto $scheme;
  X-Forwarded-For $proxy_add_x_forwarded_for;
  }
```
{: codeblock}

</br>

A anotação `response-add-headers` não suporta cabeçalhos globais para todos os serviços. Para incluir um cabeçalho para todas as respostas de serviço em um nível de servidor, é possível usar a anotação [`server-snippets`](#server-snippets):
{: tip}
```
annotations: ingress.bluemix.net/server-snippets: | add_header <header1> <value1>;
```
{: pre}
</br>

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/proxy-add-headers: |
      serviceName=<myservice1> {
      <header1> <value1>;
      <header2> <value2>;
      }
      serviceName=<myservice2> {
      <header3> <value3>;
      }
    ingress.bluemix.net/response-add-headers: |
      serviceName=<myservice1> {
      <header1>:<value1>;
      <header2>:<value2>;
      }
      serviceName=<myservice2> {
      <header3>:<value3>;
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>A chave das informações do cabeçalho para incluir na solicitação do cliente ou resposta do cliente.</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>O valor das informações do cabeçalho para incluir na solicitação do cliente ou resposta do cliente.</td>
</tr>
</tbody></table>

<br />


### Remoção do cabeçalho de resposta do cliente (`response-remove-headers`)
{: #response-remove-headers}

Remova as informações do cabeçalho que são incluídas na resposta do cliente do app final de backend antes de a resposta ser enviada para o cliente.
{:shortdesc}

**Descrição**</br> o Ingress ALB age como um proxy entre o seu app de back-end e o navegador da web do cliente. As respostas do cliente do app backend que são enviadas para o ALB são processadas (por proxy) e colocadas em uma nova resposta que é então enviada do ALB para o navegador da web do cliente. Embora a transmissão de uma resposta por proxy remova as informações do cabeçalho de HTTP inicialmente enviadas do app backend, esse processo pode não remover todos os cabeçalhos específicos do app backend. Remova as informações do cabeçalho de uma resposta do cliente antes que a resposta seja encaminhada do ALB para o navegador da web do cliente.

**YAML de recurso do Ingresso de amostra**</br>

```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/response-remove-headers: |
      serviceName=<myservice1> {
      "<header1>";
      "<header2>";
      }
      serviceName=<myservice2> {
      "<header3>";
      }
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /service1_path
        backend:
          serviceName: <myservice1>
          servicePort: 8080
      - path: /service2_path
        backend:
          serviceName: <myservice2>
          servicePort: 80
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço Kubernetes que você criou para o seu app.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>A chave do cabeçalho para remover da resposta do cliente.</td>
</tr>
</tbody></table>

<br />


### Tamanho do corpo da solicitação do cliente (`client-max-body-size`)
{: #client-max-body-size}

Configure o tamanho máximo do corpo que o cliente pode enviar como parte de uma solicitação.
{:shortdesc}

**Descrição**</br>
Para manter o desempenho esperado, o tamanho máximo do corpo da solicitação do cliente é configurado para 1 megabyte. Quando uma solicitação de cliente com um tamanho do corpo sobre o limite é enviada para o ALB de Ingresso e o cliente não permite que os dados sejam divididos, o ALB retorna uma resposta de HTTP 413 (Entidade de solicitação muito grande) ao cliente. Uma conexão entre o cliente e o ALB não é possível até que o tamanho do corpo de solicitação seja reduzido. Quando o cliente permite que os dados sejam divididos em múltiplos chunks, os dados são divididos em pacotes de 1 megabyte e enviados para o ALB.

Talvez você queira aumentar o tamanho máximo do corpo porque espera solicitações do cliente com um tamanho de corpo maior que 1 megabyte. Por exemplo, você deseja que seu cliente possa fazer upload de arquivos grandes. Aumentar o tamanho máximo do corpo de solicitação pode afetar o desempenho de seu ALB porque a conexão com o cliente deve permanecer aberta até que a solicitação seja recebida.

Alguns navegadores da web do cliente não podem exibir a mensagem de resposta HTTP 413 corretamente.
{: note}

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/client-max-body-size: "serviceName=<myservice> size=<size>; size=<size>"
spec:
 tls:
 - hosts:
   - mydomain
   secretName: mytlssecret
 rules:
 - host: mydomain
   http:
     paths:
     - path: /
       backend:
         serviceName: myservice
         servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Opcional: para aplicar um tamanho máximo do corpo do cliente a um serviço específico, substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço. Se você não especificar um nome de serviço, o tamanho será aplicado a todos os serviços. No YAML de exemplo, o formato <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> aplica o primeiro tamanho ao serviço <code>myservice</code> e aplica o segundo tamanho a todos os outros serviços.</li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>O tamanho máximo do corpo de resposta do cliente. Por exemplo, para configurar o tamanho máximo para 200 megabytes, defina <code>200m</code>. É possível configurar o tamanho como 0 para desativar a verificação do tamanho do corpo da solicitação do cliente.</td>
</tr>
</tbody></table>

<br />


## Anotações de limite de serviço
{: #service-limit}

Com as anotações de limite de serviço, é possível mudar a taxa de processamento de solicitação padrão e o número de conexões que podem vir de um único endereço IP.
{: shortdesc}

### Limites de taxa global (` global-rate-limit `)
{: #global-rate-limit}

Limite a taxa de processamento de solicitação e o número de conexões por uma chave definida para todos os serviços.
{:shortdesc}

**Descrição**</br>
Para todos os serviços, limite a taxa de processamento de solicitação e o número de conexões por uma chave definida que estejam sendo recebidas de um endereço IP único para todos os caminhos dos back-ends selecionados.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/global-rate-limit: "key=<key> rate=<rate> conn=<number-of-connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>chave</code></td>
<td>Os valores suportados são `location`, `$http_` headers e `$uri`. Para configurar um limite global para solicitações recebidas com base na zona ou no serviço, use `key=location`. Para configurar um limite global para solicitações recebidas com base no cabeçalho, use `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Substitua <code>&lt;<em>rate</em>&gt;</code> pela taxa de processamento. Insira um valor como uma taxa por segundo (r/s) ou taxa por minuto (r/m). Exemplo: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Substitua <code>&lt;<em>number-of-connections</em>&gt;</code> pelo número de conexões.</td>
</tr>
</tbody></table>

<br />


### Limites de taxa de serviço (` service-rate-limit `)
{: #service-rate-limit}

Limitar a taxa de processamento de solicitação e o número de conexões para serviços específicos.
{:shortdesc}

**Descrição**</br>
Para serviços específicos, limite a taxa de processamento de solicitação e o número de conexões por uma chave definida que estão sendo recebidas de um endereço IP único para todos os caminhos dos back-ends selecionados.

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/service-rate-limit: "serviceName=<myservice> key=<key> rate=<rate> conn=<number_of_connections>"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code>&lt;<em>myservice</em>&gt;</code> pelo nome do serviço para o qual você deseja limitar a taxa de processamento.</li>
</tr>
<tr>
<td><code>chave</code></td>
<td>Os valores suportados são `location`, `$http_` headers e `$uri`. Para configurar um limite global para solicitações recebidas com base na zona ou no serviço, use `key=location`. Para configurar um limite global para solicitações recebidas com base no cabeçalho, use `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Substitua <code>&lt;<em>rate</em>&gt;</code> pela taxa de processamento. Para definir uma taxa por segundo, use r/s: <code>10r/s</code>. Para definir uma taxa por minuto, use r/m: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Substitua <code>&lt;<em>number-of-connections</em>&gt;</code> pelo número de conexões.</td>
</tr>
</tbody></table>

<br />


## Anotações de autenticação do usuário
{: #user-authentication}

Use as anotações de autenticação do usuário se você desejar usar o {{site.data.keyword.appid_full_notm}} para autenticação com seus aplicativos.
{: shortdesc}

### {{site.data.keyword.appid_short_notm}} Autenticação (`appid-auth`)
{: #appid-auth}

Use o {{site.data.keyword.appid_full_notm}} para autenticar com seu app.
{:shortdesc}

**Descrição**</br>
Autentique solicitações HTTP/HTTPS da web ou de API com o {{site.data.keyword.appid_short_notm}}.

Se você configurar o tipo de solicitação para web, uma solicitação da web que contenha um token de acesso do {{site.data.keyword.appid_short_notm}} será validada. Se a validação do token falha, a solicitação da web é rejeitada. Se a solicitação não contém um token de acesso, a solicitação é redirecionada para a página de login do {{site.data.keyword.appid_short_notm}}. Para que a autenticação da web do {{site.data.keyword.appid_short_notm}} funcione, os cookies devem ser ativados no navegador do usuário.

Se você configurar o tipo de solicitação para API, uma solicitação de API que contenha um token de acesso do {{site.data.keyword.appid_short_notm}} será validada. Se a solicitação não contiver um token de acesso, uma mensagem de erro 401: Desautorizado será retornada para o usuário.

Por motivos de segurança, a autenticação do {{site.data.keyword.appid_short_notm}} suporta apenas back-ends com TLS/SSL ativado.
{: note}

**YAML de recurso do Ingresso de amostra**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: myingress
  annotations:
    ingress.bluemix.net/appid-auth: "bindSecret=<bind_secret> namespace=<namespace> requestType=<request_type> serviceName=<myservice> [idToken=false]"
spec:
  tls:
  - hosts:
    - mydomain
    secretName: mytlssecret
  rules:
  - host: mydomain
    http:
      paths:
      - path: /
        backend:
          serviceName: myservice
          servicePort: 8080
```
{: codeblock}

<table>
<caption>Entendendo os componentes de anotação</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Entendendo os componentes de anotação</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>Substitua <em><code>&lt;bind_secret&gt;</code></em> pelo segredo do Kubernetes que armazena o segredo de ligação para sua instância de serviço do {{site.data.keyword.appid_short_notm}}.</td>
</tr>
<tr>
<td><code> namespace</code></td>
<td>Substitua <em><code>&lt;namespace&gt;</code></em> pelo namespace do segredo de ligação. Este campo é padronizado para o namespace `padrão`.</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>Substitua <code><em>&lt;request_type&gt;</em></code> pelo tipo de solicitação que você deseja enviar para o {{site.data.keyword.appid_short_notm}}. Os valores aceitos são `web` ou `api`. O padrão é `api`.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Substitua <code><em>&lt;myservice&gt;</em></code> pelo nome do serviço Kubernetes que você criou para o seu app. Esse campo é obrigatório. Se um nome de serviço não está incluído, a anotação é ativada para todos os serviços.  Se um nome de serviço está incluído, a anotação é ativada somente para esse serviço. Separe múltiplos serviços com uma vírgula (,).</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>Opcional: o cliente Liberty OIDC não é capaz de analisar o acesso e o token de identidade ao mesmo tempo. Ao trabalhar com o Liberty, configure esse valor para false para que o token de identidade não seja enviado para o servidor Liberty.</td>
</tr>
</tbody></table>

**Uso**</br>

Como o app usa o {{site.data.keyword.appid_short_notm}} para autenticação, deve-se provisionar uma instância do {{site.data.keyword.appid_short_notm}}, configurar a instância com URIs de redirecionamento válidos e gerar um segredo de ligação, ligando a instância ao seu cluster.

1. Escolha uma instância do {{site.data.keyword.appid_short_notm}} existente ou crie uma nova.
  * Para usar uma instância existente, assegure-se de que o nome da instância de serviço não contenha espaços. Para remover espaços, selecione o menu de mais opções próximo ao nome de sua instância de serviço e selecione **Renomear serviço**.
  * Para provisionar uma [nova instância do {{site.data.keyword.appid_short_notm}}](https://cloud.ibm.com/catalog/services/app-id):
      1. Substitua o **Nome do serviço** preenchido automaticamente por seu próprio nome exclusivo para a instância de serviço. O nome da instância de serviço não pode conter espaços.
      2. Escolha a mesma região em que seu cluster está implementado.
      3. Clique em
**Criar**.

2. Inclua URLs de redirecionamento para seu app. Uma URL de redirecionamento é o terminal de retorno de chamada de seu app. Para evitar ataques de phishing, o ID do app valida a URL de solicitação com relação à lista de desbloqueio de URLs de redirecionamento.
  1. No console de gerenciamento do {{site.data.keyword.appid_short_notm}}, navegue para **Gerenciar autenticação**.
  2. Na guia **Provedores de identidade**, certifique-se de que você tenha um Provedor de identidade selecionado. Se nenhum Provedor de Identidade estiver selecionado, o usuário não será autenticado, mas um token de acesso será emitido para acesso anônimo ao app.
  3. Na guia **Configurações de autenticação**, inclua URLs de redirecionamento para seu app no formato `http://<hostname>/<app_path>/appid_callback` ou `https://<hostname>/<app_path>/appid_callback`.

    O {{site.data.keyword.appid_full_notm}} oferece uma função de logout: se `/logout` existir em seu caminho do {{site.data.keyword.appid_full_notm}}, os cookies serão removidos e o usuário será enviado de volta para a página de login. Para usar essa função, deve-se conectar `/appid_logout` ao domínio no formato `https://<hostname>/<app_path>/appid_logout` e incluir essa URL na lista de URLs de redirecionamento.
    {: note}

3. Ligue a instância de serviço do {{site.data.keyword.appid_short_notm}} ao seu cluster. O comando cria uma chave de serviço para a instância de serviço ou é possível incluir a sinalização `--key` para usar credenciais de chave de serviço existentes.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
  ```
  {: pre}
  Quando o serviço é incluído com sucesso em seu cluster, é criado um segredo de cluster que contém as credenciais de sua instância de serviço. Exemplo de saída da CLI:
  ```
  ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
    Binding service instance to namespace...
    OK
    Namespace:    mynamespace
    Secret name:  binding-<service_instance_name>
  ```
  {: screen}

4. Obtenha o segredo que foi criado em seu namespace do cluster.
  ```
  kubectl get secrets --namespace=<namespace>
  ```
  {: pre}

5. Use o segredo de ligação e o namespace do cluster para incluir a anotação `appid-auth` em seu recurso Ingress.


