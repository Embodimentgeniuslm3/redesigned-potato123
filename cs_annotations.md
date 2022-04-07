---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-18"

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


# Customizing Ingress routing with annotations
{: #ingress_annotation}

To add capabilities to your Ingress application load balancer (ALB), you can specify annotations as metadata in an Ingress resource.
{: shortdesc}

Before you use annotations, make sure that you have properly set up your Ingress service configuration by following the steps in [HTTPS load balancing with Ingress application load balancers (ALB)](/docs/containers?topic=containers-ingress). Once you have set up the Ingress ALB with a basic configuration, you can then expand its capabilities by adding annotations to the Ingress resource file.
{: note}

<table>
<caption>General annotations</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>General annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#custom-errors">Custom error actions</a></td>
<td><code>custom-errors, custom-error-actions</code></td>
<td>Indicate custom actions that the ALB can take for specific HTTP errors.</td>
</tr>
<tr>
<td><a href="#location-snippets">Location snippets</a></td>
<td><code>location-snippets</code></td>
<td>Add a custom location block configuration for a service.</td>
</tr>
<tr>
<td><a href="#alb-id">Private ALB routing</a></td>
<td><code>ALB-ID</code></td>
<td>Route incoming requests to your apps with a private ALB.</td>
</tr>
<tr>
<td><a href="#server-snippets">Server snippets</a></td>
<td><code>server-snippets</code></td>
<td>Add a custom server block configuration.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Connection annotations</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Connection annotations</th>
 <th>Name</th>
 <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#proxy-connect-timeout">Custom connect-timeouts and read-timeouts</a></td>
  <td><code>proxy-connect-timeout, proxy-read-timeout</code></td>
  <td>Set the time that the ALB waits to connect to and read from the back-end app before the back-end app is considered unavailable.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-requests">Keepalive requests</a></td>
  <td><code>keepalive-requests</code></td>
  <td>Set the maximum number of requests that can be served through one keepalive connection.</td>
  </tr>
  <tr>
  <td><a href="#keepalive-timeout">Keepalive timeout</a></td>
  <td><code>keepalive-timeout</code></td>
  <td>Set the maximum time that a keepalive connection stays open between the client and the ALB proxy server.</td>
  </tr>
  <tr>
  <td><a href="#proxy-next-upstream-config">Proxy next upstream</a></td>
  <td><code>proxy-next-upstream-config</code></td>
  <td>Set when the ALB can pass a request to the next upstream server.</td>
  </tr>
  <tr>
  <td><a href="#sticky-cookie-services">Session-affinity with cookies</a></td>
  <td><code>sticky-cookie-services</code></td>
  <td>Always route incoming network traffic to the same upstream server by using a sticky cookie.</td>
  </tr>
  <tr>
  <td><a href="#upstream-fail-timeout">Upstream fail timeout</a></td>
  <td><code>upstream-fail-timeout</code></td>
  <td>Set the amount of time during which the ALB can attempt to connect to the server before the server is considered unavailable.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive">Upstream keepalive</a></td>
  <td><code>upstream-keepalive</code></td>
  <td>Set the maximum number of idle keepalive connections for an upstream server.</td>
  </tr>
  <tr>
  <td><a href="#upstream-keepalive-timeout">Upstream keepalive timeout</a></td>
  <td><code>upstream-keepalive-timeout</code></td>
  <td>Set the maximum time that a keepalive connection stays open between the ALB proxy server and your app's upstream server.</td>
  </tr>
  <tr>
  <td><a href="#upstream-max-fails">Upstream max fails</a></td>
  <td><code>upstream-max-fails</code></td>
  <td>Set the maximum number of unsuccessful attempts to communicate with the server before the server is considered unavailable.</td>
  </tr>
  </tbody></table>

<br>

  <table>
  <caption>HTTPS and TLS/SSL authentication annotations</caption>
  <col width="20%">
  <col width="20%">
  <col width="60%">
  <thead>
  <th>HTTPS and TLS/SSL authentication annotations</th>
  <th>Name</th>
  <th>Description</th>
  </thead>
  <tbody>
  <tr>
  <td><a href="#custom-port">Custom HTTP and HTTPS ports</a></td>
  <td><code>custom-port</code></td>
  <td>Change the default ports for HTTP (port 80) and HTTPS (port 443) network traffic.</td>
  </tr>
  <tr>
  <td><a href="#redirect-to-https">HTTP redirects to HTTPS</a></td>
  <td><code>redirect-to-https</code></td>
  <td>Redirect insecure HTTP requests on your domain to HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#hsts">HTTP Strict Transport Security (HSTS)</a></td>
  <td><code>hsts</code></td>
  <td>Set the browser to access the domain only by using HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#mutual-auth">Mutual authentication</a></td>
  <td><code>mutual-auth</code></td>
  <td>Configure mutual authentication for the ALB.</td>
  </tr>
  <tr>
  <td><a href="#ssl-services">SSL services support</a></td>
  <td><code>ssl-services</code></td>
  <td>Allow SSL services support to encrypt traffic to your upstream apps that require HTTPS.</td>
  </tr>
  <tr>
  <td><a href="#tcp-ports">TCP ports</a></td>
  <td><code>tcp-ports</code></td>
  <td>Access an app via a non-standard TCP port.</td>
  </tr>
  </tbody></table>

<br>

<table>
<caption>Path routing annotations</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Path routing annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#proxy-external-service">External services</a></td>
<td><code>proxy-external-service</code></td>
<td>Add path definitions to external services, such as a service hosted in {{site.data.keyword.cloud_notm}}.</td>
</tr>
<tr>
<td><a href="#location-modifier">Location modifier</a></td>
<td><code>location-modifier</code></td>
<td>Modify the way the ALB matches the request URI against the app path.</td>
</tr>
<tr>
<td><a href="#rewrite-path">Rewrite paths</a></td>
<td><code>rewrite-path</code></td>
<td>Route incoming network traffic to a different path that your back-end app listens on.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Proxy buffer annotations</caption>
<col width="20%">
<col width="20%">
<col width="60%">
 <thead>
 <th>Proxy buffer annotations</th>
 <th>Name</th>
 <th>Description</th>
 </thead>
 <tbody>
 <tr>
<td><a href="#large-client-header-buffers">Large client header buffers</a></td>
<td><code>large-client-header-buffers</code></td>
<td>Set the maximum number and size of buffers that read large client request headers.</td>
</tr>
 <tr>
 <td><a href="#proxy-buffering">Client response data buffering</a></td>
 <td><code>proxy-buffering</code></td>
 <td>Disable the buffering of a client response on the ALB while sending the response to the client.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffers">Proxy buffers</a></td>
 <td><code>proxy-buffers</code></td>
 <td>Set the number and size of the buffers that read a response for a single connection from the proxied server.</td>
 </tr>
 <tr>
 <td><a href="#proxy-buffer-size">Proxy buffer size</a></td>
 <td><code>proxy-buffer-size</code></td>
 <td>Set the size of the buffer that reads the first part of the response that is received from the proxied server.</td>
 </tr>
 <tr>
 <td><a href="#proxy-busy-buffers-size">Proxy busy buffers size</a></td>
 <td><code>proxy-busy-buffers-size</code></td>
 <td>Set the size of proxy buffers that can be busy.</td>
 </tr>
 </tbody></table>

<br>

<table>
<caption>Request and response annotations</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Request and response annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#add-host-port">Add server port to host header</a></td>
<td><code>add-host-port</code></td>
<td>Add the server port to the host for routing requests.</td>
</tr>
<tr>
<td><a href="#client-max-body-size">Client request body size</a></td>
<td><code>client-max-body-size</code></td>
<td>Set the maximum size of the body that the client can send as part of a request.</td>
</tr>
<tr>
<td><a href="#proxy-add-headers">Additional client request or response header</a></td>
<td><code>proxy-add-headers, response-add-headers</code></td>
<td>Add header information to a client request before forwarding the request to your back-end app or to a client response before sending the response to the client.</td>
</tr>
<tr>
<td><a href="#response-remove-headers">Client response header removal</a></td>
<td><code>response-remove-headers</code></td>
<td>Remove header information from a client response before forwarding the response to the client.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>Service limit annotations</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>Service limit annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#global-rate-limit">Global rate limits</a></td>
<td><code>global-rate-limit</code></td>
<td>Limit the request processing rate and number of connections per a defined key for all services.</td>
</tr>
<tr>
<td><a href="#service-rate-limit">Service rate limits</a></td>
<td><code>service-rate-limit</code></td>
<td>Limit the request processing rate and the number of connections per a defined key for specific services.</td>
</tr>
</tbody></table>

<br>

<table>
<caption>User authentication annotations</caption>
<col width="20%">
<col width="20%">
<col width="60%">
<thead>
<th>User authentication annotations</th>
<th>Name</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><a href="#appid-auth">{{site.data.keyword.appid_short}} Authentication</a></td>
<td><code>appid-auth</code></td>
<td>Use {{site.data.keyword.appid_full}} to authenticate with your app.</td>
</tr>
</tbody></table>

<br>

## General annotations
{: #general}

### Custom error actions (`custom-errors`, `custom-error-actions`)
{: #custom-errors}

Indicate custom actions that the ALB can take for specific HTTP errors.
{: shortdesc}

**Description**</br>
To handle specific HTTP errors that might occur, you can set up custom error actions for the ALB to take.

* The `custom-errors` annotation defines the service name, the HTTP error to handle, and the name of the error action that the ALB takes when it encounters the specified HTTP error for the service.
* The `custom-error-actions` annotation defines custom error actions in NGINX code snippets.

For example, in the `custom-errors` annotation, you can set up the ALB to handle `401` HTTP errors for `app1` by returning a custom error action called `/errorAction401`. Then, in the `custom-error-actions` annotation, you can define a code snippet that is called `/errorAction401` so that the ALB returns a custom error page to the client.</br>

You can also use the `custom-errors` annotation to redirect the client to an error service that you manage. You must define the path to this error service in the `paths` section of the Ingress resource file.

**Sample Ingress resource YAML**</br>

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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>app1</em>&gt;</code> with the name of the Kubernetes service that the custom error applies to. The custom error applies only to the specific paths that use this same upstream service. If you do not set a service name, then the custom errors are applied to all service paths.</td>
</tr>
<tr>
<td><code>httpError</code></td>
<td>Replace <code>&lt;<em>401</em>&gt;</code> with the HTTP error code that you want to handle with a custom error action.</td>
</tr>
<tr>
<td><code>errorActionName</code></td>
<td>Replace <code>&lt;<em>/errorAction401</em>&gt;</code> with the name of a custom error action to take or the path to an error service.<ul>
<li>If you specify the name of a custom error action, you must define that error action in a code snippet in the <code>custom-error-actions</code> annotation. In the sample YAML, <code>app1</code> uses <code>/errorAction401</code>, which is defined in the snippet in the <code>custom-error-actions</code> annotation.</li>
<li>If you specify the path to an error service, you must specify that error path and the name of the error service in the <code>paths</code> section. In the sample YAML, <code>app2</code> uses <code>/errorPath</code>, which is defined at the end of the <code>paths</code> section.</li></ul></td>
</tr>
<tr>
<td><code>ingress.bluemix.net/custom-error-actions</code></td>
<td>Define a custom error action that the ALB takes for the service and HTTP error that you specified. Use an NGINX code snippet and end each snippet with <code>&lt;EOS&gt;</code>. In the sample YAML, the ALB passes a custom error page, <code>http://example.com/forbidden.html</code>, to the client when a <code>401</code> error occurs for <code>app1</code>.</td>
</tr>
</tbody></table>

<br />


### Location snippets (`location-snippets`)
{: #location-snippets}

Add a custom location block configuration for a service.
{:shortdesc}

**Description**</br>
A server block is an NGINX directive that defines the configuration for the ALB virtual server. A location block is an NGINX directive defined within the server block. Location blocks define how Ingress processes the request URI, or the part of the request that comes after the domain name or IP address and port.

When a server block receives a request, the location block matches the URI to a path and the request is forwarded to the IP address of the pod where the app is deployed. By using the `location-snippets` annotation, you can modify how the location block forwards requests to particular services.

To modify the server block as a whole instead, see the [`server-snippets`](#server-snippets) annotation.

To view server and location blocks in the NGINX configuration file, run the following command for one of your ALB pods: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**Sample Ingress resource YAML**</br>

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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service that you created for your app.</td>
</tr>
<tr>
<td>Location snippet</td>
<td>Provide the configuration snippet that you want to use for the specified service. The sample snippet for the <code>myservice1</code> service configures the location block to turn off proxy request buffering, turn on log rewrites, and set additional headers when it forwards a request to the service. The sample snippet for the <code>myservice2</code> service sets an empty <code>Authorization</code> header. Every location snippet must end with the value <code>&lt;EOS&gt;</code>.</td>
</tr>
</tbody></table>

<br />


### Private ALB routing (`ALB-ID`)
{: #alb-id}

Route incoming requests to your apps with a private ALB.
{:shortdesc}

**Description**</br>
Choose a private ALB to route incoming requests instead of the public ALB.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>&lt;private_ALB_ID&gt;</code></td>
<td>The ID for your private ALB. To find the private ALB ID, run <code>ibmcloud ks albs --cluster &lt;my_cluster&gt;</code>.<p>
If you have a multizone cluster with more than one private ALB enabled, you can provide a list of ALB IDs separated by <code>;</code>. For example: <code>ingress.bluemix.net/ALB-ID: &lt;private_ALB_ID_1&gt;;&lt;private_ALB_ID_2&gt;;&lt;private_ALB_ID_3&gt;</code></p>
</td>
</tr>
</tbody></table>

<br />


### Server snippets (`server-snippets`)
{: #server-snippets}

Add a custom server block configuration.
{:shortdesc}

**Description**</br>
A server block is an NGINX directive that defines the configuration for the ALB virtual server. By providing a custom configuration snippet in the `server-snippets` annotation, you can modify how the ALB handles requests at the server level.

To view server and location blocks in the NGINX configuration file, run the following command for one of your ALB pods: `kubectl exec -ti <alb_pod> -n kube-system -c nginx-ingress -- cat ./etc/nginx/default-<ingress_resource_name>.conf`
{: tip}

**Sample Ingress resource YAML**</br>

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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td>Server snippet</td>
<td>Provide the configuration snippet that you want to use. This sample snippet specifies a location block to handle <code>/health</code> requests. The location block is configured to return a healthy response and add a header when it forwards a request.</td>
</tr>
</tbody>
</table>

You can use the `server-snippets` annotation to add a header for all service responses at a server level:
{: tip}

```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: codeblock}

<br />


## Connection annotations
{: #connection}

With connection annotations, you can change how the ALB connects to the back-end app and upstream-servers, and set timeouts or a maximum number of keepalive connections before the app or server is considered to be unavailable.
{: shortdesc}

### Custom connect-timeouts and read-timeouts (`proxy-connect-timeout`, `proxy-read-timeout`)
{: #proxy-connect-timeout}

Set the time that the ALB waits to connect to and read from the back-end app before the back-end app is considered unavailable.
{:shortdesc}

**Description**</br>
When a client request is sent to the Ingress ALB, a connection to the back-end app is opened by the ALB. By default, the ALB waits 60 seconds to receive a reply from the back-end app. If the back-end app does not reply within 60 seconds, then the connection request is aborted and the back-end app is considered to be unavailable.

After the ALB is connected to the back-end app, response data is read from the back-end app by the ALB. During this read operation, the ALB waits a maximum of 60 seconds between two read operations to receive data from the back-end app. If the back-end app does not send data within 60 seconds, the connection to the back-end app is closed and the app is considered to be not available.

A 60 second connect-timeout and read-timeout are the default timeouts on a proxy and usually should not be changed.

If the availability of your app is not steady or your app is slow to respond because of high workloads, you might want to increase the connect-timeout or read-timeout. Keep in mind that increasing the timeout impacts the performance of the ALB as the connection to the back-end app must stay open until the timeout is reached.

On the other hand, you can decrease the timeout to gain performance on the ALB. Ensure that your back-end app is able to handle requests within the specified timeout, even during higher workloads.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>&lt;connect_timeout&gt;</code></td>
<td>The number of seconds or minutes to wait to connect to the back-end app, for example <code>65s</code> or <code>1m</code>. A connect-timeout cannot exceed 75 seconds.</td>
</tr>
<tr>
<td><code>&lt;read_timeout&gt;</code></td>
<td>The number of seconds or minutes to wait before the back-end app is read, for example <code>65s</code> or <code>2m</code>.
</tr>
</tbody></table>

<br />


### Keepalive requests (`keepalive-requests`)
{: #keepalive-requests}

**Description**</br>
Sets the maximum number of requests that can be served through one keepalive connection.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app. This parameter is optional. The configuration is applied to all of the services in the Ingress subdomain unless a service is specified. If the parameter is provided, the keepalive requests are set for the given service. If the parameter is not provided, the keepalive requests are set at the server level of the <code>nginx.conf</code> for all the services that do not have the keepalive requests configured.</td>
</tr>
<tr>
<td><code>requests</code></td>
<td>Replace <code>&lt;<em>max_requests</em>&gt;</code> with the maximum number of requests that can be served through one keepalive connection.</td>
</tr>
</tbody></table>

<br />


### Keepalive timeout (`keepalive-timeout`)
{: #keepalive-timeout}

**Description**</br>
Sets the maximum time that a keepalive connection stays open between the client and the ALB proxy server. If you do not use this annotation, the default timeout value is `60s`.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app. This parameter is optional. If the parameter is provided, the keepalive timeout is set for the given service. If the parameter is not provided, the keepalive timeout is set at the server level of the <code>nginx.conf</code> for all the services that do not have the keepalive timeout configured.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Replace <code>&lt;<em>time</em>&gt;</code> with an amount of time in seconds. Example: <code>timeout=20s</code>. A <code>0</code> value disables the keepalive client connections.</td>
</tr>
</tbody></table>

<br />


### Proxy next upstream (`proxy-next-upstream-config`)
{: #proxy-next-upstream-config}

Set when the ALB can pass a request to the next upstream server.
{:shortdesc}

**Description**</br>
The Ingress ALB acts as a proxy between the client app and your app. Some app setups require multiple upstream servers that handle incoming client requests from the ALB. Sometimes the proxy server that the ALB uses cannot establish a connection with an upstream server that the app uses. The ALB can then try to establish a connection with the next upstream server to pass the request to it instead. You can use the `proxy-next-upstream-config` annotation to set in which cases, for how long, and how many times the ALB can try to pass a request to the next upstream server.

Timeout is always configured when you use `proxy-next-upstream-config`, so don't add `timeout=true` to this annotation.
{: note}

**Sample Ingress resource YAML**</br>

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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>retries</code></td>
<td>Replace <code>&lt;<em>tries</em>&gt;</code> with the maximum number of times that the ALB tries to pass a request to the next upstream server. This number includes the original request. To turn off this limitation, use <code>0</code>. If you do not specify a value, the default value <code>0</code> is used.
</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Replace <code>&lt;<em>time</em>&gt;</code> with the maximum amount of time, in seconds, that the ALB tries to pass a request to the next upstream server. For example, to set a time of 30 seconds, enter <code>30s</code>. To turn off this limitation, use <code>0</code>. If you do not specify a value, the default value <code>0</code> is used.
</td>
</tr>
<tr>
<td><code>error</code></td>
<td>If set to <code>true</code>, the ALB passes a request to the next upstream server when an error occurred while establishing a connection with the first upstream server, passing a request to it, or reading the response header.
</td>
</tr>
<tr>
<td><code>invalid_header</code></td>
<td>If set to <code>true</code>, the ALB passes a request to the next upstream server when the first upstream server returns an empty or invalid response.
</td>
</tr>
<tr>
<td><code>http_502</code></td>
<td>If set to <code>true</code>, the ALB passes a request to the next upstream server when the first upstream server returns a response with the code 502. You can designate the following HTTP response codes: <code>500</code>, <code>502</code>, <code>503</code>, <code>504</code>, <code>403</code>, <code>404</code>, <code>429</code>.
</td>
</tr>
<tr>
<td><code>non_idempotent</code></td>
<td>If set to <code>true</code>, the ALB can pass requests with a non-idempotent method to the next upstream server. By default, the ALB does not pass these requests to the next upstream server.
</td>
</tr>
<tr>
<td><code>off</code></td>
<td>To prevent the ALB from passing requests to the next upstream server, set to <code>true</code>.
</td>
</tr>
</tbody></table>

<br />


### Session-affinity with cookies (`sticky-cookie-services`)
{: #sticky-cookie-services}

Use the sticky cookie annotation to add session affinity to your ALB and always route incoming network traffic to the same upstream server.
{:shortdesc}

**Description**</br>
For high availability, some app setups require you to deploy multiple upstream servers that handle incoming client requests. When a client connects to your back-end app, you can use session-affinity so that a client is served by the same upstream server during a session or for the time it takes to complete a task. You can configure your ALB to ensure session-affinity by always routing incoming network traffic to the same upstream server.

Every client that connects to your back-end app is assigned to one of the available upstream servers by the ALB. The ALB creates a session cookie that is stored in the client's app, which is included in the header information of every request between the ALB and the client. The information in the cookie ensures that all requests are handled by the same upstream server throughout the session.

Relying on sticky sessions can add complexity and reduce your availability. For example, you might have an HTTP server that maintains some session state for an initial connection so that the HTTP service accepts only subsequent requests with the same session state value. However, this prevents easy horizontal scaling of the HTTP service. Consider using an external database, such as Redis or Memcached, to store the HTTP request session value so that you can maintain the session state across multiple servers.
{: note}

When you include multiple services, use a semi-colon (;) to separate them.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>name</code></td>
<td>Replace <code>&lt;<em>cookie_name</em>&gt;</code> with the name of a sticky cookie that is created during a session.</td>
</tr>
<tr>
<td><code>expires</code></td>
<td>Replace <code>&lt;<em>expiration_time</em>&gt;</code> with the time in seconds (s), minutes (m), or hours (h) before the sticky cookie expires. This time is independent of the user activity. After the cookie is expired, the cookie is deleted by the client web browser and no longer sent to the ALB. For example, to set an expiration time of 1 second, 1 minute, or 1 hour, enter <code>1s</code>, <code>1m</code>, or <code>1h</code>.</td>
</tr>
<tr>
<td><code>path</code></td>
<td>Replace <code>&lt;<em>cookie_path</em>&gt;</code> with the path that is appended to the Ingress subdomain and that indicates for which domains and subdomains the cookie is sent to the ALB. For example, if your Ingress domain is <code>www.myingress.com</code> and you want to send the cookie in every client request, you must set <code>path=/</code>. If you want to send the cookie only for <code>www.myingress.com/myapp</code> and all its subdomains, then you must set <code>path=/myapp</code>.</td>
</tr>
<tr>
<td><code>hash</code></td>
<td>Replace <code>&lt;<em>hash_algorithm</em>&gt;</code> with the hash algorithm that protects the information in the cookie. Only <code>sha1</code> is supported. SHA1 creates a hash sum based on the information in the cookie and appends this hash sum to the cookie. The server can decrypt the information in the cookie and verify data integrity.</td>
</tr>
</tbody></table>

<br />


### Upstream fail timeout (`upstream-fail-timeout`)
{: #upstream-fail-timeout}

Set the amount of time during which the ALB can attempt to connect to the server.
{:shortdesc}

**Description**</br>
Set the amount of time during which the ALB can attempt to connect to a server before the server is considered unavailable. For a server to be considered unavailable, the ALB must hit the maximum number of failed connection attempts set by the [`upstream-max-fails`](#upstream-max-fails) annotation within the set amount of time. This amount of time also determines how long the server is considered unavailable.

**Sample Ingress resource YAML**</br>
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
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>fail-timeout</code></td>
<td>Replace <code>&lt;<em>fail_timeout</em>&gt;</code> with the amount of time that the ALB can attempt to connect to a server before the server is considered unavailable. The default is <code>10s</code>. Time must be in seconds.</td>
</tr>
</tbody></table>

<br />


### Upstream keepalive (`upstream-keepalive`)
{: #upstream-keepalive}

Set the maximum number of idle keepalive connections for an upstream server.
{:shortdesc}

**Description**</br>
Set the maximum number of idle keepalive connections to the upstream server of a given service. The upstream server has 64 idle keepalive connections by default.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>keepalive</code></td>
<td>Replace <code>&lt;<em>max_connections</em>&gt;</code> with the maximum number of idle keepalive connections to the upstream server. The default is <code>64</code>. A <code>0</code> value disables upstream keepalive connections for the given service.</td>
</tr>
</tbody></table>

<br />


### Upstream keepalive timeout (`upstream-keepalive-timeout`)
{: #upstream-keepalive-timeout}

**Description**</br>
Sets the maximum time that a keepalive connection stays open between the ALB proxy server and the upstream server for your back-end app. If you do not use this annotation, the default timeout value is `60s`.

**Sample Ingress resource YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/upstream-keepalive-timeout: "serviceName=<myservice> timeout=<time>s"
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app. This parameter is optional.</td>
</tr>
<tr>
<td><code>timeout</code></td>
<td>Replace <code>&lt;<em>time</em>&gt;</code> with an amount of time in seconds. Example: <code>timeout=20s</code>. A <code>0</code> value disables the keepalive client connections.</td>
</tr>
</tbody></table>

<br />


### Upstream max fails (`upstream-max-fails`)
{: #upstream-max-fails}

Set the maximum number of unsuccessful attempts to communicate with the server.
{:shortdesc}

**Description**</br>
Set the maximum number of times the ALB can fail to connect to the server before the server is considered unavailable. For the server to be considered unavailable, the ALB must hit the maximum number within the duration of time set by the [`upstream-fail-timeout`](#upstream-fail-timeout) annotation. The duration of time that the server is considered unavailable is also set by the `upstream-fail-timeout` annotation.

**Sample Ingress resource YAML**</br>
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
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName(Optional)</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>max-fails</code></td>
<td>Replace <code>&lt;<em>max_fails</em>&gt;</code> with the maximum number of unsuccessful attempts the ALB can make to communicate with the server. The default is <code>1</code>. A <code>0</code> value disables the annotation.</td>
</tr>
</tbody></table>

<br />


## HTTPS and TLS/SSL authentication annotations
{: #https-auth}

With HTTPS and TLS/SSL authentication annotations, you can configure your ALB for HTTPS traffic, change default HTTPS ports, enable SSL encryption for traffic that is sent to your back-end apps, or set up mutual authentication.
{: shortdesc}

### Custom HTTP and HTTPS ports (`custom-port`)
{: #custom-port}

Change the default ports for HTTP (port 80) and HTTPS (port 443) network traffic.
{:shortdesc}

**Description**</br>
By default, the Ingress ALB is configured to listen for incoming HTTP network traffic on port 80 and for incoming HTTPS network traffic on port 443. You can change the default ports to add security to your ALB domain, or to enable only an HTTPS port.

To enable mutual authentication on a port, [configure the ALB to open the valid port](/docs/containers?topic=containers-ingress-settings#opening_ingress_ports) and then specify that port in the [`mutual-auth` annotation](#mutual-auth). Do not use the `custom-port` annotation to specify a port for mutual authentication.
{: note}

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;protocol&gt;</code></td>
 <td>Enter <code>http</code> or <code>https</code> to change the default port for incoming HTTP or HTTPS network traffic.</td>
 </tr>
 <tr>
 <td><code>&lt;port&gt;</code></td>
 <td>Enter the port number to use for incoming HTTP or HTTPS network traffic.  <p class="note">When a custom port is specified for either HTTP or HTTPS, the default ports are no longer valid for both HTTP and HTTPS. For example, to change the default port for HTTPS to 8443, but use the default port for HTTP, you must set custom ports for both: <code>custom-port: "protocol=http port=80; protocol=https port=8443"</code>.</p></td>
 </tr>
</tbody></table>


**Usage**</br>
1. Review open ports for your ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  Your CLI output looks similar to the following:
  ```
  NAME                                             TYPE           CLUSTER-IP     EXTERNAL-IP    PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx 80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Open the ALB config map.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Add the non-default HTTP and HTTPS ports to the config map. Replace `<port>` with the HTTP or HTTPS port that you want to open.
  <p class="note">By default, ports 80 and 443 are open. If you want to keep 80 and 443 open, you must also include them in addition to any other TCP ports you specify in the `public-ports` field. If you enabled a private ALB, you must also specify any ports you want to keep open in the `private-ports` field. For more information, see [Opening ports in the Ingress ALB](/docs/containers?topic=containers-ingress-settings#opening_ingress_ports).</p>
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

4. Verify that your ALB is reconfigured with the non-default ports.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  Your CLI output looks similar to the following:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP    PORT(S)                           AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configure your Ingress to use the non-default ports when routing incoming network traffic to your services. Use the annotation in the sample YAML file in this reference.

6. Update your ALB configuration.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Open your preferred web browser to access your app. Example: `https://<ibmdomain>:<port>/<service_path>/`

<br />


### HTTP redirects to HTTPS (`redirect-to-https`)
{: #redirect-to-https}

Convert insecure HTTP client requests to HTTPS.
{:shortdesc}

**Description**</br>
You set up your Ingress ALB to secure your domain with the IBM-provided TLS certificate or your custom TLS certificate. Some users might try to access your apps by using an insecure `http` request to your ALB domain, for example `http://www.myingress.com`, instead of using `https`. You can use the redirect annotation to always convert insecure HTTP requests to HTTPS. If you do not use this annotation, insecure HTTP requests are not converted into HTTPS requests by default and might expose unencrypted confidential information to the public.


Redirecting HTTP requests to HTTPS is disabled by default.

**Sample Ingress resource YAML**</br>
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

**Description**</br>
HSTS instructs the browser to access a domain only by using HTTPS. Even if the user enters or follows a plain HTTP link, the browser strictly upgrades the connection to HTTPS.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>Use <code>true</code> to enable HSTS.</td>
</tr>
<tr>
<td><code>maxAge</code></td>
<td>Replace <code>&lt;<em>31536000</em>&gt;</code> with an integer that represents how many seconds a browser will cache sending requests straight to HTTPS. The default is <code>31536000</code>, which is equal to 1 year.</td>
</tr>
<tr>
<td><code>includeSubdomains</code></td>
<td>Use <code>true</code> to tell the browser that the HSTS policy also applies to all subdomains of the current domain. The default is <code>true</code>. </td>
</tr>
</tbody></table>

<br />


### Mutual authentication (`mutual-auth`)
{: #mutual-auth}

Configure mutual authentication for the ALB.
{:shortdesc}

**Description**</br>
Configure mutual authentication of downstream traffic for the Ingress ALB. The external client authenticates the server and the server also authenticates the client by using certificates. Mutual authentication is also known as certificate-based authentication or two-way authentication.

Use the `mutual-auth` annotation for SSL termination between the client and the Ingress ALB. Use the [`ssl-services` annotation](#ssl-services) for SSL termination between the Ingress ALB and the back-end app.

The mutual authentication annotation validates client certificates. To forward client certificates in a header for the applications to handle authorization, you can use the following [`proxy-add-headers` annotation](#proxy-add-headers): `"ingress.bluemix.net/proxy-add-headers": "serviceName=router-set {\n X-Forwarded-Client-Cert $ssl_client_escaped_cert;\n}\n"`
{: tip}

**Pre-requisites**</br>

* You must have a valid mutual authentication secret that contains the required `ca.crt`. To create a mutual authentication secret, see the steps at the end of this section.
* To enable mutual authentication on a port other than 443, [configure the ALB to open the valid port](/docs/containers?topic=containers-ingress-settings#opening_ingress_ports) and then specify that port in this annotation. Do not use the `custom-port` annotation to specify a port for mutual authentication.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>secretName</code></td>
<td>Replace <code>&lt;<em>mysecret</em>&gt;</code> with a name for the secret resource.</td>
</tr>
<tr>
<td><code>port</code></td>
<td>Replace <code>&lt;<em>port</em>&gt;</code> with the ALB port number.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>servicename</em>&gt;</code> with the name of one or more Ingress resources. This parameter is optional.</td>
</tr>
</tbody></table>

**To create a mutual authentication secret:**

1. Generate a certificate authority (CA) cert and key from your certificate provider. If you have your own domain, purchase an official TLS certificate for your domain. Make sure the [CN ![External link icon](../icons/launch-glyph.svg "External link icon")](https://support.dnsimple.com/articles/what-is-common-name/) is different for each certificate.
    For testing purposes, you can create a self-signed certificate by using OpenSSL. For more information, see this [self-signed SSL certificate tutorial ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.akadia.com/services/ssh_test_certificate.html) or this [mutual authentication tutorial, which includes creating your own CA ![External link icon](../icons/launch-glyph.svg "External link icon")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Convert the cert into base-64 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.base64encode.org/).
3. Create a secret YAML file by using cert.
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
4. Create the certificate as a Kubernetes secret.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### SSL services support (`ssl-services`)
{: #ssl-services}

Allow HTTPS requests and encrypt traffic to your upstream apps.
{:shortdesc}

**Description**</br>
When your Ingress resource configuration has a TLS section, the Ingress ALB can handle HTTPS-secured URL requests to your app. By default, the ALB terminates the TLS termination and decrypts the request before using the HTTP protocol to forward the traffic to your apps. If you have apps that require the HTTPS protocol and need traffic to be encrypted, use the `ssl-services` annotation. With the `ssl-services` annotation, the ALB terminates the external TLS connection, then creates a new SSL connection between the ALB and the app pod. Traffic is re-encrypted before it is sent to the upstream pods.

If your back-end app can handle TLS and you want to add additional security, you can add one-way or mutual authentication by providing a certificate that is contained in a secret.

Use the `ssl-services` annotation for SSL termination between the Ingress ALB and the back-end app. Use the [`mutual-auth` annotation](#mutual-auth) for SSL termination between the client and the Ingress ALB.
{: tip}

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>ssl-service</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service that requires HTTPS. Traffic is encrypted from the ALB to this app's service.</td>
</tr>
<tr>
<td><code>ssl-secret</code></td>
<td>If your back-end app can handle TLS and you want to add additional security, replace <code>&lt;<em>service-ssl-secret</em>&gt;</code> with the one-way or mutual authentication secret for the service.<ul><li>If you provide a one-way authentication secret, the value must contain the <code>trusted.crt</code> from the upstream server. To create a one-way secret, see the steps at the end of this section.</li><li>If you provide a mutual authentication secret, the value must contain the required <code>client.crt</code> and <code>client.key</code> that your app is expecting from the client. To create a mutual authentication secret, see the steps at the end of this section.</li></ul><p class="important">If you do not provide a secret, insecure connections are permitted. You might choose to omit a secret if want to test the connection and do not have certificates ready, or if your certificates are expired and you want to allow insecure connections.</p></td>
</tr>
</tbody></table>


**To create a one-way authentication secret:**

1. Get the certificate authority (CA) key and certificate from your upstream server and an SSL client certificate. The IBM ALB is based on NGINX, which requires the root certificate, intermediate certificate, and back-end certificate. For more information, see the [NGINX docs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.nginx.com/nginx/admin-guide/security-controls/securing-http-traffic-upstream/).
2. [Convert the cert into base-64 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.base64encode.org/).
3. Create a secret YAML file by using the cert.
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

   To also enforce mutual authentication for upstream traffic, you can provide a `client.crt` and `client.key` in addition to the `trusted.crt` in the data section.
   {: tip}

4. Create the certificate as a Kubernetes secret.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

</br>
**To create a mutual authentication secret:**

1. Generate a certificate authority (CA) cert and key from your certificate provider. If you have your own domain, purchase an official TLS certificate for your domain. Make sure the [CN ![External link icon](../icons/launch-glyph.svg "External link icon")](https://support.dnsimple.com/articles/what-is-common-name/) is different for each certificate.
    For testing purposes, you can create a self-signed certificate by using OpenSSL. For more information, see this [self-signed SSL certificate tutorial ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.akadia.com/services/ssh_test_certificate.html) or this [mutual authentication tutorial, which includes creating your own CA ![External link icon](../icons/launch-glyph.svg "External link icon")](https://blog.codeship.com/how-to-set-up-mutual-tls-authentication/).
    {: tip}
2. [Convert the cert into base-64 ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.base64encode.org/).
3. Create a secret YAML file by using the cert.
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
4. Create the certificate as a Kubernetes secret.
   ```
   kubectl create -f ssl-my-test
   ```
   {: pre}

<br />


### TCP ports (`tcp-ports`)
{: #tcp-ports}

Access an app via a non-standard TCP port.
{:shortdesc}

**Description**</br>
Use this annotation for an app that is running a TCP streams workload.

<p class="note">The ALB operates in pass-through mode and forwards traffic to back-end apps. SSL termination is not supported in this case. The TLS connection is not terminated and passes through untouched.</p>

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service to access over non-standard TCP port.</td>
</tr>
<tr>
<td><code>ingressPort</code></td>
<td>Replace <code>&lt;<em>ingress_port</em>&gt;</code> with the TCP port on which you want to access your app.</td>
</tr>
<tr>
<td><code>servicePort</code></td>
<td>This parameter is optional. When provided, the port is substituted to this value before traffic is sent to the back-end app. Otherwise, the port remains the same as the Ingress port. If you don't want to set this parameter, you can remove it from your configuration. </td>
</tr>
</tbody></table>


**Usage**</br>
1. Review open ports for your ALB.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}

  Your CLI output looks similar to the following:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                      AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   80:30416/TCP,443:32668/TCP   109d
  ```
  {: screen}

2. Open the ALB config map.
  ```
  kubectl edit configmap ibm-cloud-provider-ingress-cm -n kube-system
  ```
  {: pre}

3. Add the TCP ports to the config map. Replace `<port>` with the TCP ports that you want to open.
  By default, ports 80 and 443 are open. If you want to keep 80 and 443 open, you must also include them in addition to any other TCP ports you specify in the `public-ports` field. If you enabled a private ALB, you must also specify any ports that you want to keep open in the `private-ports` field. For more information, see [Opening ports in the Ingress ALB](/docs/containers?topic=containers-ingress-settings#opening_ingress_ports).
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

4. Verify that your ALB is re-configured with the TCP ports.
  ```
  kubectl get service -n kube-system
  ```
  {: pre}
  Your CLI output looks similar to the following:
  ```
  NAME                                             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)                               AGE
  public-cr18e61e63c6e94b658596ca93d087eed9-alb1   LoadBalancer   10.xxx.xx.xxx    169.xx.xxx.xxx   <port1>:30776/TCP,<port2>:30412/TCP   109d
  ```
  {: screen}

5. Configure the ALB to access your app via a non-standard TCP port. Use the `tcp-ports` annotation in the sample YAML file in this reference.

6. Either create your ALB resource or update your existing ALB configuration.
  ```
  kubectl apply -f myingress.yaml
  ```
  {: pre}

7. Curl the Ingress subdomain to access your app. Example: `curl <domain>:<ingressPort>`

<br />


## Path routing annotations
{: #path-routing}

The Ingress ALB routes traffic to the paths that back-end apps listen on. With path routing annotations, you can configure how the ALB routes traffic to your apps.
{: shortdesc}

### External services (`proxy-external-service`)
{: #proxy-external-service}

Add path definitions to external services, such as services hosted in {{site.data.keyword.cloud_notm}}.
{:shortdesc}

**Description**</br>
Add path definitions to external services. Use this annotation only when your app operates on an external service instead of a back-end service. When you use this annotation to create an external service route, only `client-max-body-size`, `proxy-read-timeout`, `proxy-connect-timeout`, and `proxy-buffering` annotations are supported in conjunction. Any other annotations are not supported in conjunction with `proxy-external-service`.

You cannot specify multiple hosts for a single service and path.
{: note}

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>path</code></td>
<td>Replace <code>&lt;<em>mypath</em>&gt;</code> with the path that the external service listens on.</td>
</tr>
<tr>
<td><code>external-svc</code></td>
<td>Replace <code>&lt;<em>external_service</em>&gt;</code> with the external service to be called. For example, <code>https://&lt;myservice&gt;.&lt;region&gt;.mybluemix.net</code>.</td>
</tr>
<tr>
<td><code>host</code></td>
<td>Replace <code>&lt;<em>mydomain</em>&gt;</code> with the host domain for the external service.</td>
</tr>
</tbody></table>

<br />


### Location modifier (`location-modifier`)
{: #location-modifier}

Modify the way the ALB matches the request URI against the app path.
{:shortdesc}

**Description**</br>
By default, ALBs process the paths that apps listen on as prefixes. When an ALB receives a request to an app, the ALB checks the Ingress resource for a path (as a prefix) that matches the beginning of the request URI. If a match is found, the request is forwarded to the IP address of the pod where the app is deployed.

The `location-modifier` annotation changes the way the ALB searches for matches by modifying the location block configuration. The location block determines how requests are handled for the app path.

To handle regular expression (regex) paths, this annotation is required.
{: note}

**Supported modifiers**</br>
<table>
<caption>Supported modifiers</caption>
<col width="10%">
<col width="90%">
<thead>
<th>Modifier</th>
<th>Description</th>
</thead>
<tbody>
<tr>
<td><code>=</code></td>
<td>The equal sign modifier causes the ALB to select exact matches only. When an exact match is found, the search stops and the matching path is selected.<br>For example, if your app listens on <code>/tea</code>, the ALB selects only exact <code>/tea</code> paths when matching a request to your app.</td>
</tr>
<tr>
<td><code>~</code></td>
<td>The tilde modifier causes the ALB to process paths as case-sensitive regex paths during matching.<br>For example, if your app listens on <code>/coffee</code>, the ALB can select <code>/ab/coffee</code> or <code>/123/coffee</code> paths when matching a request to your app even though the paths are not explicitly set for your app.</td>
</tr>
<tr>
<td><code>~\*</code></td>
<td>The tilde modifier that is followed by an asterisk modifier causes the ALB to process paths as case-insensitive regex paths during matching.<br>For example, if your app listens on <code>/coffee</code>, the ALB can select <code>/ab/Coffee</code> or <code>/123/COFFEE</code> paths when matching a request to your app even though the paths are not explicitly set for your app.</td>
</tr>
<tr>
<td><code>^~</code></td>
<td>The carat followed by a tilde modifier causes the ALB to select the best non-regex match instead of a regex path.</td>
</tr>
</tbody>
</table>


**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>modifier</code></td>
<td>Replace <code>&lt;<em>location_modifier</em>&gt;</code> with the location modifier you want to use for the path. Supported modifiers are <code>'='</code>, <code>'~'</code>, <code>'~\*'</code>, and <code>'^~'</code>. You must surround the modifiers in single quotes.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service you created for your app.</td>
</tr>
</tbody></table>

<br />


### Rewrite paths (`rewrite-path`)
{: #rewrite-path}

Route incoming network traffic on an ALB domain path to a different path that your back-end app listens on.
{:shortdesc}

**Description**</br>
Your Ingress ALB domain routes incoming network traffic on `mykubecluster.us-south.containers.appdomain.cloud/beans` to your app. Your app listens on `/coffee`, instead of `/beans`. To forward incoming network traffic to your app, add the rewrite annotation to your Ingress resource configuration file. The rewrite annotation ensures that incoming network traffic on `/beans` is forwarded to your app by using the `/coffee` path. When including multiple services, use only a semi-colon (;) with no space before or after the semi-colon to separate them.

**Sample Ingress resource YAML**</br>

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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>rewrite</code></td>
<td>Replace <code>&lt;<em>target_path</em>&gt;</code> with the path that your app listens on. Incoming network traffic on the ALB domain is forwarded to the Kubernetes service by using this path. Most apps do not listen on a specific path, but use the root path and a specific port. In the example for <code>mykubecluster.us-south.containers.appdomain.cloud/beans</code>, the rewrite path is <code>/coffee</code>. <p class= "note">If you apply this file and the URL shows a <code>404</code> response, your backend app might be listening on a path that ends in `/`. Try adding a trailing `/` to this rewrite field, then reapply the file and try the URL again.</p></td>
</tr>
</tbody></table>

<br />


## Proxy buffer annotations
{: #proxy-buffer}

The Ingress ALB acts as a proxy between your back-end app and the client web browser. With proxy buffer annotations, you can configure how data is buffered on your ALB when you send or receive data packets.  
{: shortdesc}

### Large client header buffers (`large-client-header-buffers`)
{: #large-client-header-buffers}

Set the maximum number and size of buffers that read large client request headers.
{:shortdesc}

**Description**</br>
Buffers that read large client request headers are allocated only by demand: If a connection is transitioned into the keepalive state after the end-of-request processing, these buffers are released. By default, there are `4` buffers and buffer size is equal to `8K` bytes. If a request line exceeds the set maximum size of one buffer, the `414 Request-URI Too Large` HTTP error is returned to the client. Additionally, if a request header field exceeds the set maximum size of one buffer, the `400 Bad Request` error is returned to the client. You can adjust the maximum number and size of buffers that are used for reading large client request headers.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
 <thead>
 <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
 </thead>
 <tbody>
 <tr>
 <td><code>&lt;number&gt;</code></td>
 <td>The maximum number of buffers that should be allocated to read large client request header. For example, to set it to 4, define <code>4</code>.</td>
 </tr>
 <tr>
 <td><code>&lt;size&gt;</code></td>
 <td>The maximum size of buffers that read large client request header. For example, to set it to 16 kilobytes, define <code>16k</code>. The size must end with a <code>k</code> for kilobyte or <code>m</code> for megabyte.</td>
 </tr>
</tbody></table>

<br />


### Client response data buffering (`proxy-buffering`)
{: #proxy-buffering}

Use the buffer annotation to disable the storage of response data on the ALB while the data is sent to the client.
{:shortdesc}

**Description**</br>
The Ingress ALB acts as a proxy between your back-end app and the client web browser. When a response is sent from the back-end app to the client, the response data is buffered on the ALB by default. The ALB proxies the client response and starts sending the response to the client at the client's pace. After all data from the back-end app is received by the ALB, the connection to the back-end app is closed. The connection from the ALB to the client remains open until the client receives all data.

If buffering of response data on the ALB is disabled, data is immediately sent from the ALB to the client. The client must be able to handle incoming data at the pace of the ALB. If the client is too slow, the upstream connection remains open until the client can catch up.

Response data buffering on the ALB is enabled by default.

**Sample Ingress resource YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/proxy-buffering: "enabled=false serviceName=<myservice1>"
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>To disable response data buffering on the ALB, set to <code>false</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code><em>&lt;myservice1&gt;</em></code> with the name of the Kubernetes service that you created for your app. Separate multiple services with a semi-colon (;). This field is optional. If you do not specify a service name, then all services use this annotation.</td>
</tr>
</tbody></table>

<br />


### Proxy buffers (`proxy-buffers`)
{: #proxy-buffers}

Configure the number and size of proxy buffers for the ALB.
{:shortdesc}

**Description**</br>
Set the number and size of the buffers that read a response for a single connection from the proxied server. The configuration is applied to all of the services in the Ingress subdomain unless a service is specified. For example, if a configuration such as `serviceName=SERVICE number=2 size=1k` is specified, 1k is applied to the service. If a configuration such as `number=2 size=1k` is specified, 1k is applied to all of the services in the Ingress subdomain.</br>
<p class="tip">If you get the error message `upstream sent too big header while reading response header from upstream`, the upstream server in your back end sent a header size that is larger than the default limit. Increase the size for both `proxy-buffers` and [`proxy-buffer-size`](#proxy-buffer-size).</p>

**Sample Ingress resource YAML**</br>

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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name for a service to apply proxy-buffers.</td>
</tr>
<tr>
<td><code>number</code></td>
<td>Replace <code>&lt;<em>number_of_buffers</em>&gt;</code> with a number, such as <code>2</code>.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Replace <code>&lt;<em>size</em>&gt;</code> with the size of each buffer in kilobytes (k or K), such as <code>1K</code>.</td>
</tr>
</tbody>
</table>

<br />


### Proxy buffer size (`proxy-buffer-size`)
{: #proxy-buffer-size}

Configure the size of the proxy buffer that reads the first part of the response.
{:shortdesc}

**Description**</br>
Set the size of the buffer that reads the first part of the response that is received from the proxied server. This part of the response usually contains a small response header. The configuration is applied to all of the services in the Ingress subdomain unless a service is specified. For example, if a configuration such as `serviceName=SERVICE size=1k` is specified, 1k is applied to the service. If a configuration such as `size=1k` is specified, 1k is applied to all of the services in the Ingress subdomain.

If you get the error message `upstream sent too big header while reading response header from upstream`, the upstream server in your back end sent a header size that is larger than the default limit. Increase the size for both `proxy-buffer-size` and [`proxy-buffers`](#proxy-buffers).
{: tip}

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of a service to apply proxy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Replace <code>&lt;<em>size</em>&gt;</code> with the size of each buffer in kilobytes (k or K), such as <code>1K</code>. To calculate the proper size, you can check out [this blog post ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.getpagespeed.com/server-setup/nginx/tuning-proxy_buffer_size-in-nginx). </td>
</tr>
</tbody></table>

<br />


### Proxy busy buffers size (`proxy-busy-buffers-size`)
{: #proxy-busy-buffers-size}

Configure the size of proxy buffers that can be busy.
{:shortdesc}

**Description**</br>
Limit the size of any buffers that are sending a response to the client while the response is not yet fully read. In the meantime, the rest of the buffers can read the response and, if needed, buffer part of the response to a temporary file. The configuration is applied to all of the services in the Ingress subdomain unless a service is specified. For example, if a configuration such as `serviceName=SERVICE size=1k` is specified, 1k is applied to the service. If a configuration such as `size=1k` is specified, 1k is applied to all of the services in the Ingress subdomain.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of a service to apply proxy-busy-buffers-size.</td>
</tr>
<tr>
<td><code>size</code></td>
<td>Replace <code>&lt;<em>size</em>&gt;</code> with the size of each buffer in kilobytes (k or K), such as <code>1K</code>.</td>
</tr>
</tbody></table>

<br />


## Request and response annotations
{: #request-response}

Use request and response annotations to add or remove header information from the client and server requests, and to change the size of the body that the client can send.
{: shortdesc}

### Add server port to host header (`add-host-port`)
{: #add-host-port}

Add a server port to the client request before the request is forwarded to your back-end app.
{: shortdesc}

**Description**</br>
Add the `:server_port` to the host header of a client request before forwarding the request to your back-end app.

**Sample Ingress resource YAML**</br>
```
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
 name: myingress
 annotations:
   ingress.bluemix.net/add-host-port: "enabled=true serviceName=<myservice>"
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>enabled</code></td>
<td>To enable setting of server_port for the subdomain, set to <code>true</code>.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code><em>&lt;myservice&gt;</em></code> with the name of the Kubernetes service that you created for your app. Separate multiple services with a semi-colon (;). This field is optional. If you do not specify a service name, then all services use this annotation.</td>
</tr>
</tbody></table>

<br />


### Additional client request or response header (`proxy-add-headers`, `response-add-headers`)
{: #proxy-add-headers}

Add extra header information to a client request before sending the request to the back-end app or to a client response before sending the response to the client.
{:shortdesc}

**Description**</br>
The Ingress ALB acts as a proxy between the client app and your back-end app. Client requests that are sent to the ALB are processed (proxied) and put into a new request that is then sent to your back-end app. Similarly, back-end app responses that are sent to the ALB are processed (proxied) and put into a new response that is then sent to the client. Proxying a request or response removes HTTP header information, such as the user name, that was initially sent from the client or back-end app.

If your back-end app requires HTTP header information, you can use the `proxy-add-headers` annotation to add header information to the client request before the request is forwarded by the ALB to the back-end app. If the client web app requires HTTP header information, you can use the `response-add-headers` annotation to add header information to the response before the response is forwarded by the ALB to the client web app.<br>

</br>

The `response-add-headers` annotation does not support global headers for all services. To add a header for all service responses at a server level, you can use the [`server-snippets` annotation](#server-snippets):
{: tip}
```
annotations:
  ingress.bluemix.net/server-snippets: |
    add_header <header1> <value1>;
```
{: pre}
</br>

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>The key of the header information to add to the client request or client response.</td>
</tr>
<tr>
<td><code>&lt;value&gt;</code></td>
<td>The value of the header information to add to the client request or client response.</td>
</tr>
</tbody></table>

<br />


### Client response header removal (`response-remove-headers`)
{: #response-remove-headers}

Remove header information that is included in the client response from the back-end end app before the response is sent to the client.
{:shortdesc}

**Description**</br>
The Ingress ALB acts as a proxy between your back-end app and the client web browser. Client responses from the back-end app that are sent to the ALB are processed (proxied), and put into a new response that is then sent from the ALB to the client web browser. Although proxying a response removes http header information that was initially sent from the back-end app, this process might not remove all back-end app specific headers. Remove header information from a client response before the response is forwarded from the ALB to the client web browser.

**Sample Ingress resource YAML**</br>

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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>service_name</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the Kubernetes service that you created for your app.</td>
</tr>
<tr>
<td><code>&lt;header&gt;</code></td>
<td>The key of the header to remove from the client response.</td>
</tr>
</tbody></table>

<br />


### Client request body size (`client-max-body-size`)
{: #client-max-body-size}

Set the maximum size of the body that the client can send as part of a request.
{:shortdesc}

**Description**</br>
To maintain the expected performance, the maximum client request body size is set to 1 megabyte. When a client request with a body size over the limit is sent to the Ingress ALB, and the client does not allow data to be divided, the ALB returns a 413 (Request Entity Too Large) HTTP response to the client. A connection between the client and the ALB is not possible until the size of the request body is reduced. When the client allows data to be split up into multiple chunks, data is divided into packages of 1 megabyte and sent to the ALB.

You might want to increase the maximum body size because you expect client requests with a body size that is greater than 1 megabyte. For example, you want your client to be able to upload large files. Increasing the maximum request body size might impact the performance of your ALB because the connection to the client must stay open until the request is received.

Some client web browsers cannot display the 413 HTTP response message properly.
{: note}

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Optional: To apply a client max body size to a specific service, replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service. If you do not specify a service name, the size is applied to all services. In the example YAML, the format <code>"serviceName=&lt;myservice&gt; size=&lt;size&gt;; size=&lt;size&gt;"</code> applies the first size to the <code>myservice</code> service and applies the second size to all other services.</li>
</tr>
<td><code>&lt;size&gt;</code></td>
<td>The maximum size of the client response body. For example, to set the maximum size to 200 megabytes, define <code>200m</code>. You can set the size to 0 to disable the check of the client request body size.</td>
</tr>
</tbody></table>

<br />


## Service limit annotations
{: #service-limit}

With service limit annotations, you can change the default request processing rate and the number of connections that can come from a single IP address.
{: shortdesc}

### Global rate limits (`global-rate-limit`)
{: #global-rate-limit}

Limit the request processing rate and number of connections per a defined key for all services.
{:shortdesc}

**Description**</br>
For all services, limit the request processing rate and the number of connections per a defined key that are coming from a single IP address for all paths of the selected back ends.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>key</code></td>
<td>Supported values are `location`, `$http_` headers, and `$uri`. To set a global limit for incoming requests based on the zone or service, use `key=location`. To set a global limit for incoming requests based on the header, use `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Replace <code>&lt;<em>rate</em>&gt;</code> with the processing rate. Enter a value as a rate per second (r/s) or rate per minute (r/m). Example: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Replace <code>&lt;<em>number-of-connections</em>&gt;</code> with the number of connections.</td>
</tr>
</tbody></table>

<br />


### Service rate limits (`service-rate-limit`)
{: #service-rate-limit}

Limit the request processing rate and the number of connections for specific services.
{:shortdesc}

**Description**</br>
For specific services, limit the request processing rate and the number of connections per a defined key that are coming from a single IP address for all paths of the selected back ends.

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code>&lt;<em>myservice</em>&gt;</code> with the name of the service for which you want to limit the processing rate.</li>
</tr>
<tr>
<td><code>key</code></td>
<td>Supported values are `location`, `$http_` headers, and `$uri`. To set a global limit for incoming requests based on the zone or service, use `key=location`. To set a global limit for incoming requests based on the header, use `X-USER-ID key=$http_x_user_id`.</td>
</tr>
<tr>
<td><code>rate</code></td>
<td>Replace <code>&lt;<em>rate</em>&gt;</code> with the processing rate. To define a rate per second, use r/s: <code>10r/s</code>. To define a rate per minute, use r/m: <code>50r/m</code>.</td>
</tr>
<tr>
<td><code>conn</code></td>
<td>Replace <code>&lt;<em>number-of-connections</em>&gt;</code> with the number of connections.</td>
</tr>
</tbody></table>

<br />


## User authentication annotations
{: #user-authentication}

Use user authentication annotations if you want to use {{site.data.keyword.appid_full_notm}} to authenticate with your apps.
{: shortdesc}

### {{site.data.keyword.appid_short_notm}} Authentication (`appid-auth`)
{: #appid-auth}

Use {{site.data.keyword.appid_full_notm}} to authenticate with your app.
{:shortdesc}

**Description**</br>
Authenticate web or API HTTP/HTTPS requests with {{site.data.keyword.appid_short_notm}}.

If you set the request type to web, a web request that contains an {{site.data.keyword.appid_short_notm}} access token is validated. If token validation fails, the web request is rejected. If the request does not contain an access token, then the request is redirected to the {{site.data.keyword.appid_short_notm}} login page. For {{site.data.keyword.appid_short_notm}} web authentication to work, cookies must be enabled in the user's browser.

If you set the request type to api, an API request that contains an {{site.data.keyword.appid_short_notm}} access token is validated. If the request does not contain an access token, a 401: Unauthorized error message is returned to the user.

For security reasons, {{site.data.keyword.appid_short_notm}} authentication supports only back ends with TLS/SSL enabled.
{: note}

**Sample Ingress resource YAML**</br>
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
<caption>Understanding the annotation components</caption>
<thead>
<th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the annotation components</th>
</thead>
<tbody>
<tr>
<td><code>bindSecret</code></td>
<td>Replace <em><code>&lt;bind_secret&gt;</code></em> with the Kubernetes secret, which stores the bind secret for your {{site.data.keyword.appid_short_notm}} service instance.</td>
</tr>
<tr>
<td><code>namespace</code></td>
<td>Replace <em><code>&lt;namespace&gt;</code></em> with the namespace of the bind secret. This field defaults to the `default` namespace.</td>
</tr>
<tr>
<td><code>requestType</code></td>
<td>Replace <code><em>&lt;request_type&gt;</em></code> with the type of request you want to send to {{site.data.keyword.appid_short_notm}}. Accepted values are `web` or `api`. The default is `api`.</td>
</tr>
<tr>
<td><code>serviceName</code></td>
<td>Replace <code><em>&lt;myservice&gt;</em></code> with the name of the Kubernetes service that you created for your app. This field is required. If a service name is not included, then the annotation is enabled for all services.  If a service name is included, then the annotation is enabled only for that service. Separate multiple services with a comma (,).</td>
</tr>
<tr>
<td><code>idToken=false</code></td>
<td>Optional: The Liberty OIDC client is unable to parse both the access and the identity token at the same time. When working with Liberty, set this value to false so that the identity token is not sent to the Liberty server.</td>
</tr>
</tbody></table>

**Usage**</br>

Because the app uses {{site.data.keyword.appid_short_notm}} for authentication, you must provision an {{site.data.keyword.appid_short_notm}} instance, configure the instance with valid redirect URIs, and generate a bind secret by binding the instance to your cluster.

1. Choose an existing or create a new {{site.data.keyword.appid_short_notm}} instance.
  * To use an existing instance, ensure that the service instance name doesn't contain spaces. To remove spaces, select the more options menu next to the name of your service instance and select **Rename service**.
  * To provision a [new {{site.data.keyword.appid_short_notm}} instance](https://cloud.ibm.com/catalog/services/app-id):
      1. Replace the auto-filled **Service name** with your own unique name for the service instance. The service instance name can't contain spaces.
      2. Choose the same region that your cluster is deployed in.
      3. Click **Create**.

2. Add redirect URLs for your app. A redirect URL is the callback endpoint of your app. To prevent phishing attacks, App ID validates the request URL against the whitelist of redirect URLs.
  1. In the {{site.data.keyword.appid_short_notm}} management console, navigate to **Manage Authentication**.
  2. In the **Identity providers** tab, make sure that you have an Identity Provider selected. If no Identity Provider is selected, the user will not be authenticated but will be issued an access token for anonymous access to the app.
  3. In the **Authentication settings** tab, add redirect URLs for your app in the format `http://<hostname>/<app_path>/appid_callback` or `https://<hostname>/<app_path>/appid_callback`.

    {{site.data.keyword.appid_full_notm}} offers a logout function: If `/logout` exists in your {{site.data.keyword.appid_full_notm}} path, cookies are removed and the user is sent back to the login page. To use this function, you must append `/appid_logout` to your domain in the format `https://<hostname>/<app_path>/appid_logout` and include this URL in the redirect URLs list.
    {: note}

3. Bind the {{site.data.keyword.appid_short_notm}} service instance to your cluster. The command creates a service key for the service instance, or you can include the `--key` flag to use existing service key credentials.
  ```
  ibmcloud ks cluster-service-bind --cluster <cluster_name_or_ID> --namespace <namespace> --service <service_instance_name> [--key <service_instance_key>]
  ```
  {: pre}
  When the service is successfully added to your cluster, a cluster secret is created that holds the credentials of your service instance. Example CLI output:
  ```
  ibmcloud ks cluster-service-bind --cluster mycluster --namespace mynamespace --service appid1
  Binding service instance to namespace...
  OK
  Namespace:    mynamespace
  Secret name:  binding-<service_instance_name>
  ```
  {: screen}

4. Get the secret that was created in your cluster namespace.
  ```
  kubectl get secrets --namespace=<namespace>
  ```
  {: pre}

5. Use the bind secret and the cluster namespace to add the `appid-auth` annotation to your Ingress resource.


