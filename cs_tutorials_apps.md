---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-12"

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


# Tutorial: Deploying apps into Kubernetes clusters
{: #cs_apps_tutorial}

You can learn how to use {{site.data.keyword.containerlong}} to deploy a containerized app that leverages {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}}.
{: shortdesc}

In this scenario, a fictional PR firm uses the {{site.data.keyword.cloud_notm}} service to analyze their press releases and receive feedback on the tone of their messages.

Using the Kubernetes cluster that is created in the last tutorial, the PR firm's app developer deploys a Hello World version of the app. Building on each lesson in this tutorial, the app developer deploys progressively more complicated versions of the same app. The following diagram shows the components of each deployment by lesson.

<img src="images/cs_app_tutorial_mz-roadmap.png" width="700" alt="Lesson components" style="width:700px; border-style: none"/>

As depicted in the diagram, Kubernetes uses several different types of resources to get your apps up and running in clusters. In Kubernetes, deployments and services work together. Deployments include the definitions for the app. For example, the image to use for the container and which port must be exposed for the app. When you create a deployment, a Kubernetes pod is created for each container that you defined in the deployment. To make your app more resilient, you can define multiple instances of the same app in your deployment and let Kubernetes automatically create a replica set for you. The replica set monitors the pods and assures that the specified number of pods are always up and running. If one of the pods becomes unresponsive, the pod is re-created automatically.

Services group a set of pods and provide network connection to these pods for other services in the cluster without exposing the actual private IP address of each pod. You can use Kubernetes services to make an app available to other pods inside the cluster or to expose an app to the internet. In this tutorial, you use a Kubernetes service to access your running app from the internet by using a public IP address that is automatically assigned to a worker node and a public port.

To make your app even more highly available, in standard clusters, you can create a worker pool that spans multiple zones with worker nodes in each zone to run even more replicas of your app. This task is not covered in this tutorial, but keep this concept in mind for future improvements to an app's availability.

Only one of the lessons includes the integration of an {{site.data.keyword.cloud_notm}} service into an app, but you can use them with as simple or complex of an app as you can imagine.

## Objectives
{: #apps_objectives}

* Understand basic Kubernetes terminology
* Push an image to your registry namespace in {{site.data.keyword.registryshort_notm}}
* Make an app publicly accessible
* Deploy a single instance of an app in a cluster by using a Kubernetes command and a script
* Deploy multiple instances of an app in containers that are re-created during health checks
* Deploy an app that uses functionality from an {{site.data.keyword.cloud_notm}} service

## Time required
{: #apps_time}

40 minutes

## Audiences
{: #apps_audience}

Software developers and network administrators that are deploying an app into a Kubernetes cluster for the first time.

## Prerequisites
{: #apps_prereqs}

[Tutorial: Creating Kubernetes clusters](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial)


## Lesson 1: Deploying single instance apps to Kubernetes clusters
{: #cs_apps_tutorial_lesson1}

In the previous tutorial, you created a cluster with one worker node. In this lesson, you configure a deployment and deploy a single instance of the app into a Kubernetes pod within the worker node.
{:shortdesc}

The components that you deploy by completing this lesson are shown in the following diagram.

![Deployment setup](images/cs_app_tutorial_mz-components1.png)

To deploy the app:

1.  Clone the source code for the [Hello world app ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM/container-service-getting-started-wt) to your user home directory. The repository contains different versions of a similar app in folders that each start with `Lab`. Each version contains the following files:
    * `Dockerfile`: The build definitions for the image.
    * `app.js`: The Hello world app.
    * `package.json`: Metadata about the app.

    ```
    git clone https://github.com/IBM/container-service-getting-started-wt.git
    ```
    {: pre}

2.  Navigate to the `Lab 1` directory.

    ```
    cd 'container-service-getting-started-wt/Lab 1'
    ```
    {: pre}

3. [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

5.  Log in to the {{site.data.keyword.registryshort_notm}} CLI.

    ```
    ibmcloud cr login
    ```
    {: pre}
    -   If you forgot your namespace in {{site.data.keyword.registryshort_notm}}, run the following command.

        ```
        ibmcloud cr namespace-list
        ```
        {: pre}

6.  Build a Docker image that includes the app files of the `Lab 1` directory, and push the image to the {{site.data.keyword.registryshort_notm}} namespace that you created in the previous tutorial. If you need to make a change to the app in the future, repeat these steps to create another version of the image. **Note**: Learn more about [securing your personal information](/docs/containers?topic=containers-security#pi) when you work with container images.

    Use lowercase alphanumeric characters or underscores (`_`) only in the image name. Don't forget the period (`.`) at the end of the command. The period tells Docker to look inside the current directory for the Dockerfile and build artifacts to build the image. To get the registry region that you are currently in, run `ibmcloud cr region`.

    ```
    ibmcloud cr build -t <region>.icr.io/<namespace>/hello-world:1 .
    ```
    {: pre}

    When the build is complete, verify that you see the following success message:

    ```
    Successfully built <image_ID>
    Successfully tagged <region>.icr.io/<namespace>/hello-world:1
    The push refers to a repository [<region>.icr.io/<namespace>/hello-world]
    29042bc0b00c: Pushed
    f31d9ee9db57: Pushed
    33c64488a635: Pushed
    0804854a4553: Layer already exists
    6bd4a62f5178: Layer already exists
    9dfa40a0da3b: Layer already exists
    1: digest: sha256:f824e99435a29e55c25eea2ffcbb84be4b01345e0a3efbd7d9f238880d63d4a5 size: 1576
    ```
    {: screen}

7.  Deployments are used to manage pods, which include containerized instances of an app. The following command deploys the app in a single pod. For the purposes of this tutorial, the deployment is named **hello-world-deployment**, but you can give the deployment any name that you want.

    ```
    kubectl create deployment hello-world-deployment --image=<region>.icr.io/<namespace>/hello-world:1
    ```
    {: pre}

    Example output:

    ```
    deployment "hello-world-deployment" created
    ```
    {: screen}

    Learn more about [securing your personal information](/docs/containers?topic=containers-security#pi) when you work with Kubernetes resources.

8.  Make the app accessible to the world by exposing the deployment as a NodePort service. Just as you might expose a port for a Cloud Foundry app, the NodePort that you expose is the port on which the worker node listens for traffic.

    ```
    kubectl expose deployment/hello-world-deployment --type=NodePort --port=8080 --name=hello-world-service --target-port=8080
    ```
    {: pre}

    Example output:

    ```
    service "hello-world-service" exposed
    ```
    {: screen}

    <table summary=“Information about the expose command parameters.”>
    <caption>More about the expose parameters</caption>
    <thead>
    <th colspan=2><img src="images/idea.png" alt="Idea icon"/> More about the expose parameters</th>
    </thead>
    <tbody>
    <tr>
    <td><code>expose</code></td>
    <td>Expose a resource as a Kubernetes service and make it publicly available to users.</td>
    </tr>
    <tr>
    <td><code>deployment/<em>&lt;hello-world-deployment&gt;</em></code></td>
    <td>The resource type and the name of the resource to expose with this service.</td>
    </tr>
    <tr>
    <td><code>--name=<em>&lt;hello-world-service&gt;</em></code></td>
    <td>The name of the service.</td>
    </tr>
    <tr>
    <td><code>--port=<em>&lt;8080&gt;</em></code></td>
    <td>The port on which the service serves.</td>
    </tr>
    <tr>
    <td><code>--type=NodePort</code></td>
    <td>The service type to create.</td>
    </tr>
    <tr>
    <td><code>--target-port=<em>&lt;8080&gt;</em></code></td>
    <td>The port to which the service directs traffic. In this instance, the target-port is the same as the port, but other apps you create might differ.</td>
    </tr>
    </tbody></table>

9. Now that all the deployment work is done, you can test your app in a browser. Get the details to form the URL.
    1.  Get information about the service to see which NodePort was assigned.

        ```
        kubectl describe service hello-world-service
        ```
        {: pre}

        Example output:

        ```
        Name:                   hello-world-service
        Namespace:              default
        Labels:                 run=hello-world-deployment
        Selector:               run=hello-world-deployment
        Type:                   NodePort
        IP:                     10.xxx.xx.xxx
        Port:                   <unset> 8080/TCP
        NodePort:               <unset> 30872/TCP
        Endpoints:              172.30.xxx.xxx:8080
        Session Affinity:       None
        No events.
        ```
        {: screen}

        The NodePorts are randomly assigned when they are generated with the `expose` command, but within 30000-32767. In this example, the NodePort is 30872.

    2.  Get the public IP address for the worker node in the cluster.

        ```
        ibmcloud ks workers --cluster <cluster_name_or_ID>
        ```
        {: pre}

        Example output:

        ```
        ibmcloud ks workers --cluster pr_firm_cluster
        Listing cluster workers...
        OK
        ID                                                 Public IP       Private IP       Machine Type   State    Status   Zone   Version
        kube-mil01-pa10c8f571c84d4ac3b52acbf50fd11788-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    free           normal   Ready    mil01      1.13.8
        ```
        {: screen}

10. Open a browser and check out the app with the following URL: `http://<IP_address>:<NodePort>`. With the example values, the URL is `http://169.xx.xxx.xxx:30872`. When you enter that URL in a browser, you can see the following text.

    ```
    Hello world! Your app is up and running in a cluster!
    ```
    {: screen}

    To see that the app is publicly available, try entering it into a browser on your cell phone.
    {: tip}

11. [Launch the Kubernetes dashboard](/docs/containers?topic=containers-app#cli_dashboard).

    If you select your cluster in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/), you can use the **Kubernetes Dashboard** button to launch your dashboard with one click.
    {: tip}

12. In the **Workloads** tab, you can see the resources that you created.

Good job! You deployed your first version of the app.

Too many commands in this lesson? Agreed. How about using a configuration script to do some of the work for you? To use a configuration script for the second version of the app, and to create higher availability by deploying multiple instances of that app, continue to the next lesson.

<br />


## Lesson 2: Deploying and updating apps with higher availability
{: #cs_apps_tutorial_lesson2}

In this lesson, you deploy three instances of the Hello World app into a cluster for higher availability than the first version of the app.
{:shortdesc}

Higher availability means that user access is divided across the three instances. When too many users are trying to access the same app instance, they might notice slow response times. Multiple instances can mean faster response times for your users. In this lesson, you also learn how health checks and deployment updates can work with Kubernetes. The following diagram includes the components that you deploy by completing this lesson.

![Deployment setup](images/cs_app_tutorial_mz-components2.png)

In the previous tutorial, you created your account and a cluster with one worker node. In this lesson, you configure a deployment and deploy three instances of the Hello World app. Each instance is deployed in a Kubernetes pod as part of a replica set in the worker node. To make it publicly available, you also create a Kubernetes service.

As defined in the configuration script, Kubernetes can use an availability check to see whether a container in a pod is running or not. For example, these checks might catch deadlocks, where an app is running, but it is unable to progress. Restarting a container that is in this condition can help to make the app more available despite bugs. Then, Kubernetes uses a readiness check to know when a container is ready to start accepting traffic again. A pod is considered ready when its container is ready. When the pod is ready, it is started again. In this version of the app, every 15 seconds it times out. With a health check configured in the configuration script, containers are re-created if the health check finds an issue with an app.

1.  In a CLI, navigate to the `Lab 2` directory.

  ```
  cd 'container-service-getting-started-wt/Lab 2'
  ```
  {: pre}

2.  If you started a new CLI session, log in and set the cluster context.

3.  Build, tag, and push the app as an image to your namespace in {{site.data.keyword.registryshort_notm}}.  Again, don't forget the period (`.`) at the end of the command.

    ```
    ibmcloud cr build -t <region>.icr.io/<namespace>/hello-world:2 .
      ```
    {: pre}

    Verify that you see the success message.

    ```
    Successfully built <image_ID>
    Successfully tagged <region>.icr.io/<namespace>/hello-world:1
    The push refers to a repository [<region>.icr.io/<namespace>/hello-world]
    29042bc0b00c: Pushed
    f31d9ee9db57: Pushed
    33c64488a635: Pushed
    0804854a4553: Layer already exists
    6bd4a62f5178: Layer already exists
    9dfa40a0da3b: Layer already exists
    1: digest: sha256:f824e99435a29e55c25eea2ffcbb84be4b01345e0a3efbd7d9f238880d63d4a5 size: 1576
    ```
    {: screen}

4.  Open the `healthcheck.yml` file, in the `Lab 2` directory, with a text editor. This configuration script combines a few steps from the previous lesson to create a deployment and a service at the same time. The PR firm's app developers can use these scripts when updates are made or to troubleshoot issues by re-creating the pods.
    1. Update the details for the image in your private registry namespace.

        ```
        image: "<region>.icr.io/<namespace>/hello-world:2"
        ```
        {: codeblock}

    2.  In the **Deployment** section, note the `replicas`. Replicas are the number instances of your app. Running three instances makes the app more highly available than just one instance.

        ```
        replicas: 3
        ```
        {: codeblock}

    3.  Note the HTTP liveness probe that checks the health of the container every 5 seconds.

        ```
        livenessProbe:
                    httpGet:
                      path: /healthz
                      port: 8080
                    initialDelaySeconds: 5
                    periodSeconds: 5
        ```
        {: codeblock}

    4.  In the **Service** section, note the `NodePort`. Rather than generating a random NodePort like you did in the previous lesson, you can specify a port in the 30000 - 32767 range. This example uses 30072.

5.  Switch back to the CLI that you used to set your cluster context and run the configuration script. When the deployment and the service are created, the app is available for the PR firm's users to see.

  ```
  kubectl apply -f healthcheck.yml
  ```
  {: pre}

  Example output:

  ```
  deployment "hw-demo-deployment" created
  service "hw-demo-service" created
  ```
  {: screen}

6.  Now that the deployment work is done you can open a browser and check out the app. To form the URL, take the same public IP address that you used in the previous lesson for your worker node and combine it with the NodePort that was specified in the configuration script. To get the public IP address for the worker node:

  ```
  ibmcloud ks workers --cluster <cluster_name_or_ID>
  ```
  {: pre}

  With the example values, the URL is `http://169.xx.xxx.xxx:30072`. In a browser, you might see the following text. If you do not see this text, don't worry. This app is designed to go up and down.

  ```
  Hello world! Great job getting the second stage up and running!
  ```
  {: screen}

  You can also check `http://169.xx.xxx.xxx:30072/healthz` for status.

  For the first 10 - 15 seconds, a 200 message is returned, so you know that the app is running successfully. After those 15 seconds, a timeout message is displayed. This is an expected behavior.

  ```
  {
    "error": "Timeout, Health check error!"
  }
  ```
  {: screen}

7.  Check your pod status to monitor the health of your app in Kubernetes. You can check the status from the CLI or in the Kubernetes dashboard.

    *  **From the CLI**: Watch what is happening to your pods as they change status.
       ```
       kubectl get pods -o wide -w
       ```
       {: pre}

    *  **From the Kubernetes dashboard**:

       1.  [Launch the Kubernetes dashboard](/docs/containers?topic=containers-app#cli_dashboard).
       2.  In the **Workloads** tab, you can see the resources that you created. From this tab, you can continually refresh and see that the health check is working. In the **Pods** section, you can see how many times the pods are restarted when the containers in them are re-created. If you happen to catch the following error in the dashboard, this message indicates that the health check caught a problem. Give it a few minutes and refresh again. You see the number of restarts changes for each pod.

       ```
       Liveness probe failed: HTTP probe failed with statuscode: 500
       Back-off restarting failed docker container
       Error syncing pod, skipping: failed to "StartContainer" for "hw-container" with CrashLoopBackOff: "Back-off 1m20s restarting failed container=hw-container pod=hw-demo-deployment-3090568676-3s8v1_default(458320e7-059b-11e7-8941-56171be20503)"
       ```
       {: screen}

Good job! You deployed the second version of the app. You had to use fewer commands, learned how health checks work, and edited a deployment, which is great! The Hello world app passed the test for the PR firm. Now, you can deploy a more useful app for the PR firm to start analyzing press releases.

Ready to delete what you created before you continue? This time, you can use the same configuration script to delete both of the resources that you created.

  ```
  kubectl delete -f healthcheck.yml
  ```
  {: pre}

  Example output:

  ```
  deployment "hw-demo-deployment" deleted
  service "hw-demo-service" deleted
  ```
  {: screen}

<br />


## Lesson 3: Deploying and updating the Watson Tone Analyzer app
{: #cs_apps_tutorial_lesson3}

In the previous lessons, the apps were deployed as single components in one worker node. In this lesson, you can deploy two components of an app into a cluster that use the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} service.
{:shortdesc}

Separating components into different containers ensures that you can update one without affecting the others. Then, you update the app to scale it up with more replicas to make it more highly available. The following diagram includes the components that you deploy by completing this lesson.

![Deployment setup](images/cs_app_tutorial_mz-components3.png)

From the previous tutorial, you have your account and a cluster with one worker node. In this lesson, you create an instance of {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} service in your {{site.data.keyword.cloud_notm}} account and configure two deployments, one deployment for each component of the app. Each component is deployed in a Kubernetes pod in the worker node. To make both of those components publicly available, you also create a Kubernetes service for each component.


### Lesson 3a: Deploying the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} app
{: #lesson3a}

1.  In a CLI, navigate to the `Lab 3` directory.

  ```
  cd 'container-service-getting-started-wt/Lab 3'
  ```
  {: pre}

2.  If you started a new CLI session, log in and set the cluster context.

3.  Build the first {{site.data.keyword.watson}} image.

    1.  Navigate to the `watson` directory.

        ```
        cd watson
        ```
        {: pre}

    2.  Build, tag, and push the `watson` app as an image to your namespace in {{site.data.keyword.registryshort_notm}}. Again, don't forget the period (`.`) at the end of the command.

        ```
        ibmcloud cr build -t <region>.icr.io/<namespace>/watson .
        ```
        {: pre}

        Verify that you see the success message.

        ```
        Successfully built <image_id>
        ```
        {: screen}

4.  Build the {{site.data.keyword.watson}}-talk image.

    1.  Navigate to the `watson-talk` directory.

        ```
        cd 'container-service-getting-started-wt/Lab 3/watson-talk'
        ```
        {: pre}

    2.  Build, tag, and push the `watson-talk`app as an image to your namespace in {{site.data.keyword.registryshort_notm}}. Again, don't forget the period (`.`) at the end of the command.

        ```
        ibmcloud cr build -t <region>.icr.io/<namespace>/watson-talk .
        ```
        {: pre}

        Verify that you see the success message.

        ```
        Successfully built <image_id>
        ```
        {: screen}

5.  Verify that the images were successfully added to your registry namespace.

    ```
    ibmcloud cr images
    ```
    {: pre}

    Example output:

    ```
    Listing images...

    REPOSITORY                        NAMESPACE  TAG      DIGEST         CREATED         SIZE     VULNERABILITY STATUS
    us.icr.io/namespace/hello-world   namespace  1        0d90cb732881   40 minutes ago  264 MB   OK
    us.icr.io/namespace/hello-world   namespace  2        c3b506bdf33e   20 minutes ago  264 MB   OK
    us.icr.io/namespace/watson        namespace  latest   fedbe587e174   3 minutes ago   274 MB   OK
    us.icr.io/namespace/watson-talk   namespace  latest   fedbe587e174   2 minutes ago   274 MB   OK
    ```
    {: screen}

6.  Open the `watson-deployment.yml` file, in the `Lab 3` directory, with a text editor. This configuration script includes a deployment and a service for both the `watson` and `watson-talk` components of the app.

    1.  Update the details for the image in your registry namespace for both deployments.

        watson:

        ```
        image: "<region>.icr.io/namespace/watson"
        ```
        {: codeblock}

        watson-talk:

        ```
        image: "<region>.icr.io/namespace/watson-talk"
        ```
        {: codeblock}

    2.  In the volumes section of the `watson-pod` deployment, update the name of the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} secret that you created in the previous [Creating Kubernetes cluster tutorial](/docs/containers?topic=containers-cs_cluster_tutorial#cs_cluster_tutorial_lesson4). By mounting the Kubernetes secret as a volume to your deployment, you make the {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) API key available to the container that is running in your pod. The {{site.data.keyword.watson}} app components in this tutorial are configured to look up the API key by using the volume mount path.

        ```
        volumes:
                - name: service-bind-volume
                  secret:
                    defaultMode: 420
                    secretName: binding-mytoneanalyzer
        ```
        {: codeblock}

        If you forgot what you named the secret, run the following command.

        ```
        kubectl get secrets --namespace=default
        ```
        {: pre}

    3.  In the watson-talk service section, note the value that is set for the `NodePort`. This example uses 30080.

7.  Run the configuration script.

  ```
  kubectl apply -f watson-deployment.yml
  ```
  {: pre}

8.  Optional: Verify that the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} secret is mounted as a volume to the pod.

    1.  To get the name of a watson pod, run the following command.

        ```
        kubectl get pods
        ```
        {: pre}

        Example output:

        ```
        NAME                                 READY     STATUS    RESTARTS  AGE
        watson-pod-4255222204-rdl2f          1/1       Running   0         13h
        watson-talk-pod-956939399-zlx5t      1/1       Running   0         13h
        ```
        {: screen}

    2.  Get the details about the pod and look for the secret name.

        ```
        kubectl describe pod <pod_name>
        ```
        {: pre}

        Example output:

        ```
        Volumes:
          service-bind-volume:
            Type:       Secret (a volume populated by a Secret)
            SecretName: binding-mytoneanalyzer
          default-token-j9mgd:
            Type:       Secret (a volume populated by a Secret)
            SecretName: default-token-j9mgd
        ```
        {: codeblock}

9.  Open a browser and analyze some text. The format of the URL is `http://<worker_node_IP_address>:<watson-talk-nodeport>/analyze/"<text_to_analyze>"`.

    Example:

    ```
    http://169.xx.xxx.xxx:30080/analyze/"Today is a beautiful day"
    ```
    {: screen}

    In a browser, you can see the JSON response for the text you entered.

10. [Launch the Kubernetes dashboard](/docs/containers?topic=containers-app#cli_dashboard).

11. In the **Workloads** tab, you can see the resources that you created.

### Lesson 3b. Updating the running Watson Tone Analyzer deployment
{: #lesson3b}

While a deployment is running, you can edit the deployment to change values in the pod template. This lesson includes updating the image that is used. The PR firm wants to change the app in the deployment.
{: shortdesc}

Change the name of the image:

1.  Open the configuration details for the running deployment.

    ```
    kubectl edit deployment/watson-talk-pod
    ```
    {: pre}

    Depending on your operating system, either a vi editor opens or a text editor opens.

2.  Change the name of the image to the ibmliberty image.

    ```
    spec:
          containers:
          - image: <region>.icr.io/ibmliberty:latest
    ```
    {: codeblock}

3.  Save your changes and exit the editor.

4.  Apply the changes to the running deployment.

    ```
    kubectl rollout status deployment/watson-talk-pod
    ```
    {: pre}

    Wait for confirmation that the rollout is complete.

    ```
    deployment "watson-talk-pod" successfully rolled out
    ```
    {: screen}

    When you roll out a change, another pod is created and tested by Kubernetes. When the test is successful, the old pod is removed.

[Test your knowledge and take a quiz! ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibmcloud-quizzes.mybluemix.net/containers/apps_tutorial/quiz.php)

Good job! You deployed the {{site.data.keyword.watson}} {{site.data.keyword.toneanalyzershort}} app. The PR firm can start to use this deployment to start analyzing their press releases.

Ready to delete what you created? You can use the configuration script to delete the resources that you created.

  ```
  kubectl delete -f watson-deployment.yml
  ```
  {: pre}

  Example output:

  ```
  deployment "watson-pod" deleted
  deployment "watson-talk-pod" deleted
  service "watson-service" deleted
  service "watson-talk-service" deleted
  ```
  {: screen}

  If you do not want to keep the cluster, you can delete that too.

  ```
  ibmcloud ks cluster-rm --cluster <cluster_name_or_ID>
  ```
  {: pre}

## What's next?
{: #apps_next}

Now that you conquered the basics, you can move to more advanced activities. Consider trying out one of the following:

- Complete a [more complicated lab ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/IBM/container-service-getting-started-wt#lab-overview) in the repository
- [Automatically scale your apps](/docs/containers?topic=containers-app#app_scaling) with {{site.data.keyword.containerlong_notm}}
- Explore the container orchestration code patterns on [IBM Developer ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/technologies/containers/)
