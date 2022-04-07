---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-20"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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



# Setting up the CLI and API
{: #cs_cli_install}

You can use the {{site.data.keyword.containerlong}} CLI or API to create and manage your Kubernetes clusters.
{:shortdesc}

## Installing the IBM Cloud CLI and plug-ins
{: #cs_cli_install_steps}

Install the required CLIs to create and manage your Kubernetes clusters in {{site.data.keyword.containerlong_notm}}, and to deploy containerized apps to your cluster.
{:shortdesc}

This task includes the information for installing these CLIs and plug-ins:

-   {{site.data.keyword.cloud_notm}} CLI
-   {{site.data.keyword.containerlong_notm}} plug-in
-   {{site.data.keyword.registryshort_notm}} plug-in

If you want to use the {{site.data.keyword.cloud_notm}} console instead, after your cluster is created, you can run CLI commands directly from your web browser in the [Kubernetes Terminal](#cli_web).
{: tip}

<br>
To install the CLIs:

1.  Install the [{{site.data.keyword.cloud_notm}} CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/cli?topic=cloud-cli-getting-started#idt-prereq). This installation includes:
    -   The base {{site.data.keyword.cloud_notm}} CLI (`ibmcloud`).
    -   The {{site.data.keyword.containerlong_notm}} plug-in (`ibmcloud ks`).
    -   {{site.data.keyword.registryshort_notm}} plug-in (`ibmcloud cr`). Use this plug-in to set up your own namespace in a multi-tenant, highly available, and scalable private image registry that is hosted by IBM, and to store and share Docker images with other users. Docker images are required to deploy containers into a cluster.
    -   The Kubernetes CLI (`kubectl`) that matches the default version: 1.13.8.<p class="note">If you plan to use a cluster that runs a different version, you might need to [install that version of the Kubernetes CLI separately](#kubectl). If you have an (OpenShift) cluster, you [install the `oc` and `kubectl` CLIs together](#cli_oc).</p>
    -   The Helm CLI (`helm`). You might use Helm as a package manager to install {{site.data.keyword.cloud_notm}} services and complex apps to your cluster via Helm charts. You must still [set up Helm](/docs/containers?topic=containers-helm) in each cluster where you want to use Helm.

    Plan to use the CLI often? Try [Enabling shell autocompletion for {{site.data.keyword.cloud_notm}} CLI (Linux/MacOS only)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
    ```
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}

3.  Verify that the {{site.data.keyword.containerlong_notm}} plug-in and {{site.data.keyword.registryshort_notm}} plug-ins are installed correctly.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    Example output:
    ```
    Listing installed plug-ins...

    Plugin Name                            Version   Status        
    container-registry                     0.1.373     
    container-service/kubernetes-service   0.3.23   
    ```
    {: screen}

For reference information about these CLIs, see the documentation for those tools.

-   [`ibmcloud` commands](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [`ibmcloud ks` commands](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli)
-   [`ibmcloud cr` commands](/docs/services/Registry?topic=container-registry-cli-plugin-containerregcli)

## Installing the Kubernetes CLI (`kubectl`)
{: #kubectl}

To view a local version of the Kubernetes dashboard and to deploy apps into your clusters, install the Kubernetes CLI (`kubectl`). The latest stable version of `kubectl` is installed with the base {{site.data.keyword.cloud_notm}} CLI. However, to work with your cluster, you must instead install the Kubernetes CLI `major.minor` version that matches the Kubernetes cluster `major.minor` version that you plan to use. If you use a `kubectl` CLI version that does not match at least the `major.minor` version of your clusters, you might experience unexpected results. For example, [Kubernetes does not support ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/setup/version-skew-policy/) `kubectl` client versions that are 2 or more versions apart from the server version (n +/- 2). Make sure to keep your Kubernetes cluster and CLI versions up-to-date.
{: shortdesc}

Using an OpenShift cluster? [Install the OpenShift Origin CLI (`oc`)](#cli_oc). If you have both Red Hat OpenShift on IBM Cloud and Ubuntu {{site.data.keyword.containershort_notm}} clusters, make sure to use the `kubectl` binary file that matches your cluster `major.minor` Kubernetes version. You might want to set up multiple directories on your local machine to organize different `kubectl` versions and then create aliases in your terminal for these directories.
{: tip}

1.  If you already have a cluster, check that the version of your client `kubectl` CLI matches the version of the cluster API server.
    1.  [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)
    2.  Compare the client and server versions. If the client does not match the server, continue to the next step. If the versions match, you already installed the appropriate version of `kubectl`.
        ```
        kubectl version --short
        ```
        {: pre}
2.  Download the Kubernetes CLI `major.minor` version that matches the Kubernetes cluster `major.minor` version that you plan to use. The current {{site.data.keyword.containerlong_notm}} default Kubernetes version is 1.13.8.
    -   **OS X**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.8/bin/darwin/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.13.8/bin/darwin/amd64/kubectl)
    -   **Linux**: [https://storage.googleapis.com/kubernetes-release/release/v1.13.8/bin/linux/amd64/kubectl ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.13.8/bin/linux/amd64/kubectl)
    -   **Windows**: Install the Kubernetes CLI in the same directory as the {{site.data.keyword.cloud_notm}} CLI. This setup saves you some file path changes when you run commands later. [https://storage.googleapis.com/kubernetes-release/release/v1.13.8/bin/windows/amd64/kubectl.exe ![External link icon](../icons/launch-glyph.svg "External link icon")](https://storage.googleapis.com/kubernetes-release/release/v1.13.8/bin/windows/amd64/kubectl.exe)

3.  If you use OS X or Linux, complete the following steps.
    1.  Move the executable file to the `/usr/local/bin` directory.
        ```
        mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Make sure that `/usr/local/bin` is listed in your `PATH` system variable. The `PATH` variable contains all directories where your operating system can find executable files. The directories that are listed in the `PATH` variable serve different purposes. `/usr/local/bin` is used to store executable files for software that is not part of the operating system and that was manually installed by the system administrator.
        ```
        echo $PATH
        ```
        {: pre}
        Example CLI output:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}

    3.  Make the file executable.
        ```
        chmod +x /usr/local/bin/kubectl
        ```
        {: pre}
4.  If you have clusters that run different versions of Kubernetes, such as 1.14.4 and 1.12.10, download each `kubectl` version binary file to a separate directory. Then, you can set up an alias in your local terminal profile to point to the `kubectl` binary file directory that matches the `kubectl` version of the cluster that you want to work with, or [run the CLI from a container](#cs_cli_container).
5.  **Optional**: [Enable autocompletion for `kubectl` commands ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). The steps vary depending on the shell that you use.

Next, start [Creating Kubernetes clusters from the CLI with {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters#clusters_cli_steps).

For more information about the Kubernetes CLI, see the [`kubectl` reference docs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubectl.docs.kubernetes.io/).
{: note}

<br />


## Installing the OpenShift Origin CLI (`oc`) preview beta
{: #cli_oc}

[Red Hat OpenShift on IBM Cloud](/docs/containers?topic=containers-openshift_tutorial) is available as a beta to test out OpenShift clusters.
{: preview}

To view a local version of the OpenShift dashboard and to deploy apps into your Red Hat OpenShift on IBM Cloud clusters, install the OpenShift Origin CLI (`oc`). The `oc` CLI includes a matching version of the Kubernetes CLI (`kubectl`). For more information, see the [OpenShift docs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.openshift.com/container-platform/3.11/cli_reference/get_started_cli.html).
{: shortdesc}

Using both community Kubernetes and OpenShift clusters? The `oc` CLI comes with both the `oc` and `kubectl` binaries, but your different clusters might run different versions of Kubernetes, such as 1.11 on OpenShift and 1.13.8 on Ubuntu. Make sure to use the `kubectl` binary that matches your cluster `major.minor` Kubernetes version.
{: note}

1.  [Download the OpenShift Origin CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://www.okd.io/download.html) for your local operating system and OpenShift version. The current default OpenShift version is 3.11.

2.  If you use Mac OS or Linux, complete the following steps to add the binaries to your `PATH` system variable. If you use Windows, install the `oc` CLI in the same directory as the {{site.data.keyword.cloud_notm}} CLI. This setup saves you some file path changes when you run commands later.
    1.  Move the `oc` and `kubectl` executable files to the `/usr/local/bin` directory.
        ```
        mv /<filepath>/oc /usr/local/bin/oc && mv /<filepath>/kubectl /usr/local/bin/kubectl
        ```
        {: pre}

    2.  Make sure that `/usr/local/bin` is listed in your `PATH` system variable. The `PATH` variable contains all directories where your operating system can find executable files. The directories that are listed in the `PATH` variable serve different purposes. `/usr/local/bin` is used to store executable files for software that is not part of the operating system and that was manually installed by the system administrator.
        ```
        echo $PATH
        ```
        {: pre}
        Example CLI output:
        ```
        /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
        ```
        {: screen}
3.  If you have clusters that run different versions of Kubernetes, such as an OpenShift cluster with version 1.11 and a community Kubernetes cluster with version 1.14.4, download each `kubectl` version binary file to a separate directory.
    1.  Delete the `kubectl` binary file that comes with the `oc` installation, because this `kubectl` version does not work with community Kubernetes clusters.
        ```
        rm /usr/local/bin/kubectl
        ```
        {: pre}
    2.  [Download separate `kubectl` binary files](#kubectl) that match the versions of your OpenShift and community Kubernetes clusters.
    3.  **Optional**: Set up an alias in your local terminal profile to point to separate binaries that match the version of `kubectl` your cluster needs.
4.  **Optional**: [Enable autocompletion for `kubectl` commands ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion). The steps vary depending on the shell that you use. You can repeat the steps to enable autocompletion for `oc` commands. For example in bash on Linux, instead of `kubectl completion bash >/etc/bash_completion.d/kubectl`, you can run `oc completion bash >/etc/bash_completion.d/oc_completion`.

Next, start [Creating a Red Hat OpenShift on IBM Cloud cluster (preview)](/docs/containers?topic=containers-openshift_tutorial).

For more information about the OpenShift Origin CLI, see the [`oc` commands docs ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.openshift.com/container-platform/3.11/cli_reference/basic_cli_operations.html).
{: note}

<br />


## Running the CLI in a container on your computer
{: #cs_cli_container}

Instead of installing each of the CLIs individually on your computer, you can install the CLIs into a container that runs on your computer.
{:shortdesc}

Before you begin, [install Docker for Mac ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/docker-for-mac/install/) or [Windows ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/docker-for-windows/install/) to build and run images locally. If you are using Windows 8 or earlier, you can install the [Docker Toolbox ![External link icon](../icons/launch-glyph.svg "External link icon")](https://docs.docker.com/toolbox/toolbox_install_windows/) instead.

1. Create an image from the provided Dockerfile.

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Deploy the image locally as a container and mount a volume to access local files.

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. Begin running `ibmcloud ks` and `kubectl` commands from the interactive shell. If you create data that you want to save, save that data to the volume that you mounted. When you exit the shell, the container stops.

<br />



## Configuring the CLI to run `kubectl`
{: #cs_cli_configure}

You can use the commands that are provided with the Kubernetes CLI to manage clusters in {{site.data.keyword.cloud_notm}}.
{:shortdesc}

All `kubectl` commands that are available in Kubernetes 1.13.8 are supported for use with clusters in {{site.data.keyword.cloud_notm}}. After you create a cluster, set the context for your local CLI to that cluster with an environment variable. Then, you can run the Kubernetes `kubectl` commands to work with your cluster in {{site.data.keyword.cloud_notm}}.

Before you can run `kubectl` commands:
* [Install the required CLIs](#cs_cli_install).
* [Create a cluster](/docs/containers?topic=containers-clusters#clusters_cli_steps).
* Make sure that you have a [service role](/docs/containers?topic=containers-users#platform) that grants the appropriate Kubernetes RBAC role so that you can work with Kubernetes resources. If you have only a service role but no platform role, you need the cluster admin to give you the cluster name and ID, or the **Viewer** platform role to list clusters.

To use `kubectl` commands:

1.  Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.

    ```
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}

2.  Select an {{site.data.keyword.cloud_notm}} account. If you are assigned to multiple {{site.data.keyword.cloud_notm}} organizations, select the organization where the cluster was created. Clusters are specific to an organization, but are independent from an {{site.data.keyword.cloud_notm}} space. Therefore, you are not required to select a space.

3.  To create and work with clusters in a resource group other than the default, target that resource group. To see the resource group that each cluster belongs to, run `ibmcloud ks clusters`. **Note**: You must have [**Viewer** access](/docs/containers?topic=containers-users#platform) to the resource group.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  List all of the clusters in the account to get the name of the cluster. If you have only an {{site.data.keyword.cloud_notm}} IAM service role and cannot view clusters, ask your cluster admin for the IAM platform **Viewer** role, or the cluster name and ID.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

     To work with free clusters in the London metro location, you must target the EU Central regional API by running `ibmcloud ks init --host https://eu-gb.containers.cloud.ibm.com`.
     {: note}

5.  Set the cluster you created as the context for this session. Complete these configuration steps every time that you work with your cluster.
    1.  Get the command to set the environment variable and download the Kubernetes configuration files. <p class="tip">Using Windows PowerShell? Include the `--powershell` flag to get environment variables in Windows PowerShell format.</p>
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

        After downloading the configuration files, a command is displayed that you can use to set the path to the local Kubernetes configuration file as an environment variable.

        Example:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/kubernetes-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copy and paste the command that is displayed in your terminal to set the `KUBECONFIG` environment variable.

        **Mac or Linux users**: Instead of running the `ibmcloud ks cluster-config` command and copying the `KUBECONFIG` environment variable, you can run `ibmcloud ks cluster-config --export <cluster-name>`. Depending on your shell, you can set up your shell by running `eval $(ibmcloud ks cluster-config --export <cluster-name>)`.
        {: tip}

    3.  Verify that the `KUBECONFIG` environment variable is set properly.

        Example:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Output:
        ```
        /Users/<user_name>/.bluemix/plugins/kubernetes-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

6.  Verify that the `kubectl` commands run properly with your cluster by checking the Kubernetes CLI server version.

    ```
    kubectl version  --short
    ```
    {: pre}

    Example output:

    ```
    Client Version: v1.13.8
    Server Version: v1.13.8
    ```
    {: screen}

Now, you can run `kubectl` commands to manage your clusters in {{site.data.keyword.cloud_notm}}. For a full list of commands, see the [Kubernetes documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubectl.docs.kubernetes.io/).

**Tip:** If you are using Windows and the Kubernetes CLI is not installed in the same directory as the {{site.data.keyword.cloud_notm}} CLI, you must change directories to the path where the Kubernetes CLI is installed to run `kubectl` commands successfully.


<br />


## Accessing an OpenShift cluster from the terminal or automation tools
{: #openshift_cluster_login}

Red Hat OpenShift on IBM Cloud is integrated with {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) so that you can authenticate users and services by using their IAM identities and authorize actions with access roles and policies. When you authenticate as a user through the OpenShift console, your IAM identity is used to generate an OpenShift login token that you can use to log in to the terminal. If you want to automate logging in to your cluster, you can create an IAM API key to use for the `oc login` command.
{:shortdesc}

**Before you begin**:
* [Install the `oc` CLI](#cli_oc).
* [Create an OpenShift cluster](/docs/containers?topic=containers-openshift_tutorial).

<br>
**To automate access to your cluster with an API key**:
1.  Create an {{site.data.keyword.cloud_notm}} API key.<p class="important">Save your API key in a secure location. You cannot retrieve the API key again. If you want to export the output to a file on your local machine, include the `--file <path>/<file_name>` flag.</p>
    ```
    ibmcloud iam api-key-create <name>
    ```
    {: pre}
2.  Get the **Master URL** of the cluster that you want to access. 
    ```
    ibmcloud ks cluster-get --cluster <cluster_name_or_ID>
    ```
    {: pre}
3.  Use the API key and cluster URL to log in to your OpenShift cluster. The user name (`-u`) is `apikey`, the password (`-p`) is your API key value, and the `--server` is the service endpoint URL of the cluster master.
    ```
    oc login -u apikey -p <API_key> --server=<master_URL>
    ```
    {: pre}

<br>
**To log in to your cluster as a user through the terminal**:
1.  In the [{{site.data.keyword.containerlong_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/kubernetes/clusters), click the cluster that you want to access.
2.  Click the **Access** tab and follow the instructions.

<br />


## Updating the CLI
{: #cs_cli_upgrade}

You might want to update the CLIs periodically to use new features.
{:shortdesc}

This task includes the information for updating these CLIs.

-   {{site.data.keyword.cloud_notm}} CLI version 0.8.0 or later
-   {{site.data.keyword.containerlong_notm}} plug-in
-   Kubernetes CLI version 1.13.8 or later
-   {{site.data.keyword.registryshort_notm}} plug-in

<br>
To update the CLIs:

1.  Update the {{site.data.keyword.cloud_notm}} CLI. Download the [latest version ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/cli?topic=cloud-cli-getting-started) and run the installer.

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.

    ```
    ibmcloud login
    ```
    {: pre}

     If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
     {: tip}

3.  Update the {{site.data.keyword.containerlong_notm}} plug-in.
    1.  Install the update from the {{site.data.keyword.cloud_notm}} plug-in repository.

        ```
        ibmcloud plugin update kubernetes-service 
        ```
        {: pre}

    2.  Verify the plug-in installation by running the following command and checking the list of the plug-ins that are installed.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        The {{site.data.keyword.containerlong_notm}} plug-in is displayed in the results as kubernetes-service.

    3.  Initialize the CLI.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Update the Kubernetes CLI](#kubectl).

5.  Update the {{site.data.keyword.registryshort_notm}} plug-in.
    1.  Install the update from the {{site.data.keyword.cloud_notm}} plug-in repository.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  Verify the plug-in installation by running the following command and checking the list of the plug-ins that are installed.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        The registry plug-in is displayed in the results as container-registry.

<br />


## Uninstalling the CLI
{: #cs_cli_uninstall}

If you no longer need the CLI, you can uninstall it.
{:shortdesc}

This task includes the information for removing these CLIs:


-   {{site.data.keyword.containerlong_notm}} plug-in
-   Kubernetes CLI
-   {{site.data.keyword.registryshort_notm}} plug-in

To uninstall the CLIs:

1.  Uninstall the {{site.data.keyword.containerlong_notm}} plug-in.

    ```
    ibmcloud plugin uninstall kubernetes-service
    ```
    {: pre}

2.  Uninstall the {{site.data.keyword.registryshort_notm}} plug-in.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  Verify the plug-ins were uninstalled by running the following command and checking the list of the plug-ins that are installed.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    The kubernetes-service and the container-registry plug-in are not displayed in the results.

<br />


## Using the Kubernetes Terminal in your web browser (beta)
{: #cli_web}

The Kubernetes Terminal allows you to use the {{site.data.keyword.cloud_notm}} CLI to manage your cluster directly from your web browser.
{: shortdesc}

The Kubernetes Terminal is released as a beta {{site.data.keyword.containerlong_notm}} add-on and might change due to user feedback and further tests. Do not use this feature in production clusters to avoid unexpected side effects.
{: important}

If you use the cluster dashboard in the {{site.data.keyword.cloud_notm}} console to manage your clusters but want to quickly make more advanced configuration changes, you can now run CLI commands directly from your web browser in the Kubernetes Terminal. The Kubernetes Terminal is enabled with the base [{{site.data.keyword.cloud_notm}} CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/cli?topic=cloud-cli-getting-started), the {{site.data.keyword.containerlong_notm}} plug-in, and the {{site.data.keyword.registryshort_notm}} plug-in. Additionally, the terminal context is already set to the cluster that you are working with so that you can run Kubernetes `kubectl` commands to work with your cluster.

Any files that you download and edit locally, such as YAML files, are stored temporarily in Kubernetes Terminal and do not persist across sessions.
{: note}

To install and launch the Kubernetes Terminal:

1.  Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/).
2.  From the menu bar, select the account that you want to use.
3.  From the menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon"), click **Kubernetes**.
4.  On the **Clusters** page, click the cluster that you want to access.
5.  From the cluster detail page, click the **Terminal** button.
6.  Click **Install**. It might take a few minutes for the terminal add-on to install.
7.  Click the **Terminal** button again. The terminal opens in your browser.

Next time, you can launch the Kubernetes Terminal simply by clicking the **Terminal** button.

<br />


## Automating cluster deployments with the API
{: #cs_api}

You can use the {{site.data.keyword.containerlong_notm}} API to automate the creation, deployment, and management of your Kubernetes clusters.
{:shortdesc}

The {{site.data.keyword.containerlong_notm}} API requires header information that you must provide in your API request and that can vary depending on the API that you want to use. To determine what header information is needed for your API, see the [{{site.data.keyword.containerlong_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://us-south.containers.cloud.ibm.com/swagger-api).

To authenticate with {{site.data.keyword.containerlong_notm}}, you must provide an {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) token that is generated with your {{site.data.keyword.cloud_notm}} credentials and that includes the {{site.data.keyword.cloud_notm}} account ID where the cluster was created. Depending on the way you authenticate with {{site.data.keyword.cloud_notm}}, you can choose between the following options to automate the creation of your {{site.data.keyword.cloud_notm}} IAM token.

You can also use the [API swagger JSON file ![External link icon](../icons/launch-glyph.svg "External link icon")](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json) to generate a client that can interact with the API as part of your automation work.
{: tip}

<table summary="ID types and options with the input parameter in column 1 and the value in column 2.">
<caption>ID types and options</caption>
<thead>
<th>{{site.data.keyword.cloud_notm}} ID</th>
<th>My options</th>
</thead>
<tbody>
<tr>
<td>Unfederated ID</td>
<td><ul><li><strong>Generate an {{site.data.keyword.cloud_notm}} API key:</strong> As an alternative to using the {{site.data.keyword.cloud_notm}} user name and password, you can <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">use {{site.data.keyword.cloud_notm}} API keys</a>. {{site.data.keyword.cloud_notm}} API keys are dependent on the {{site.data.keyword.cloud_notm}} account they are generated for. You cannot combine your {{site.data.keyword.cloud_notm}} API key with a different account ID in the same {{site.data.keyword.cloud_notm}} IAM token. To access clusters that were created with an account other than the one your {{site.data.keyword.cloud_notm}} API key is based on, you must log in to the account to generate a new API key.</li>
<li><strong>{{site.data.keyword.cloud_notm}} user name and password:</strong> You can follow the steps in this topic to fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM access token.</li></ul>
</tr>
<tr>
<td>Federated ID</td>
<td><ul><li><strong>Generate an {{site.data.keyword.cloud_notm}} API key:</strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">{{site.data.keyword.cloud_notm}} API keys</a> are dependent on the {{site.data.keyword.cloud_notm}} account they are generated for. You cannot combine your {{site.data.keyword.cloud_notm}} API key with a different account ID in the same {{site.data.keyword.cloud_notm}} IAM token. To access clusters that were created with an account other than the one your {{site.data.keyword.cloud_notm}} API key is based on, you must log in to the account to generate a new API key.</li>
<li><strong>Use a one-time passcode: </strong> If you authenticate with {{site.data.keyword.cloud_notm}} by using a one-time passcode, you cannot fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM token because the retrieval of your one-time passcode requires a manual interaction with your web browser. To fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM token, you must create an {{site.data.keyword.cloud_notm}} API key instead.</ul></td>
</tr>
</tbody>
</table>

1.  Create your {{site.data.keyword.cloud_notm}} IAM access token. The body information that is included in your request varies based on the {{site.data.keyword.cloud_notm}} authentication method that you use.

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Input parameters to retrieve IAM tokens with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters to get IAM tokens.</caption>
    <thead>
        <th>Input parameters</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>Content-Type: application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Note</strong>: <code>Yng6Yng=</code> equals the URL-encoded authorization for the user name <strong>bx</strong> and the password <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} user name and password</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Your {{site.data.keyword.cloud_notm}} user name.</li>
    <li>`password`: Your {{site.data.keyword.cloud_notm}} password.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br><strong>Note</strong>: Add the <code>uaa_client_secret</code> key with no value specified.</li></ul></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} API keys</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Your {{site.data.keyword.cloud_notm}} API key</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Note</strong>: Add the <code>uaa_client_secret</code> key with no value specified.</li></ul></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} one-time passcode</td>
      <td><ul><li><code>grant_type: urn:ibm:params:oauth:grant-type:passcode</code></li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Your {{site.data.keyword.cloud_notm}} one-time passcode. Run `ibmcloud login --sso` and follow the instructions in your CLI output to retrieve your one-time passcode by using your web browser.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Note</strong>: Add the <code>uaa_client_secret</code> key with no value specified.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    Example output for using an API key:

    ```
    {
    "access_token": "<iam_access_token>",
    "refresh_token": "<iam_refresh_token>",
    "uaa_token": "<uaa_token>",
    "uaa_refresh_token": "<uaa_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1493747503
    "scope": "ibm openid"
    }

    ```
    {: screen}

    You can find the {{site.data.keyword.cloud_notm}} IAM token in the **access_token** field of your API output. Note the {{site.data.keyword.cloud_notm}} IAM token to retrieve additional header information in the next steps.

2.  Retrieve the ID of the {{site.data.keyword.cloud_notm}} account that you want to work with. Replace `<iam_access_token>` with the {{site.data.keyword.cloud_notm}} IAM token that you retrieved from the **access_token** field of your API output in the previous step. In your API output, you can find the ID of your {{site.data.keyword.cloud_notm}} account in the **resources.metadata.guid** field.

    ```
    GET https://accountmanagement.ng.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Input parameters to get {{site.data.keyword.cloud_notm}} account ID with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters to get an {{site.data.keyword.cloud_notm}} account ID.</caption>
    <thead>
  	<th>Input parameters</th>
  	<th>Values</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Headers</td>
      <td><ul><li><code>Content-Type: application/json</code></li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li><code>Accept: application/json</code></li></ul></td>
  	</tr>
    </tbody>
    </table>

    Example output:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

3.  Generate a new {{site.data.keyword.cloud_notm}} IAM token that includes your {{site.data.keyword.cloud_notm}} credentials and the account ID that you want to work with.

    If you use an {{site.data.keyword.cloud_notm}} API key, you must use the {{site.data.keyword.cloud_notm}} account ID the API key was created for. To access clusters in other accounts, log into this account and create an {{site.data.keyword.cloud_notm}} API key that is based on this account.
    {: note}

    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Input parameters to retrieve IAM tokens with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters to get IAM tokens.</caption>
    <thead>
        <th>Input parameters</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>Note</strong>: <code>Yng6Yng=</code> equals the URL-encoded authorization for the user name <strong>bx</strong> and the password <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} user name and password</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Your {{site.data.keyword.cloud_notm}} user name. </li>
    <li>`password`: Your {{site.data.keyword.cloud_notm}} password. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Note</strong>: Add the <code>uaa_client_secret</code> key with no value specified.</li>
    <li>`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step.</li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} API keys</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Your {{site.data.keyword.cloud_notm}} API key.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Note</strong>: Add the <code>uaa_client_secret</code> key with no value specified.</li>
    <li>`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step.</li></ul>
      </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} one-time passcode</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Your {{site.data.keyword.cloud_notm}} passcode. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br><strong>Note</strong>: Add the <code>uaa_client_secret</code> key with no value specified.</li>
    <li>`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step.</li></ul></td>
    </tr>
    </tbody>
    </table>

    Example output:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    You can find the {{site.data.keyword.cloud_notm}} IAM token in the **access_token** and the refresh token in the **refresh_token** field of your API output.

4.  List available {{site.data.keyword.containerlong_notm}} regions and select the region that you want to work in. Use the IAM access token and refresh token from the previous step to build your header information.
    ```
    GET https://containers.cloud.ibm.com/v1/regions
    ```
    {: codeblock}

    <table summary="Input parameters to retrieve {{site.data.keyword.containerlong_notm}} regions with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters to retrieve {{site.data.keyword.containerlong_notm}} regions.</caption>
    <thead>
    <th>Input parameters</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Authorization: bearer <iam_token>`</li>
    <li>`X-Auth-Refresh-Token: <refresh_token>`</li></ul></td>
    </tr>
    </tbody>
    </table>

    Example output:
    ```
    {
    "regions": [
        {
            "name": "ap-north",
            "alias": "jp-tok",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "ap-south",
            "alias": "au-syd",
            "cfURL": "api.au-syd.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "eu-central",
            "alias": "eu-de",
            "cfURL": "api.eu-de.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "uk-south",
            "alias": "eu-gb",
            "cfURL": "api.eu-gb.bluemix.net",
            "freeEnabled": true
        },
        {
            "name": "us-east",
            "alias": "us-east",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": false
        },
        {
            "name": "us-south",
            "alias": "us-south",
            "cfURL": "api.ng.bluemix.net",
            "freeEnabled": true
        }
    ]
    }
    ```
    {: screen}

5.  List all clusters in the {{site.data.keyword.containerlong_notm}} region that you selected. If you want to [run Kubernetes API requests against your cluster](#kube_api), make sure to note the **id** and **region** of your cluster.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="Input parameters to work with the {{site.data.keyword.containerlong_notm}} API with the input parameter in column 1 and the value in column 2.">
     <caption>Input parameters to work with the {{site.data.keyword.containerlong_notm}} API.</caption>
     <thead>
     <th>Input parameters</th>
     <th>Values</th>
     </thead>
     <tbody>
     <tr>
     <td>Header</td>
     <td><ul><li>`Authorization: bearer <iam_token>`</li>
       <li>`X-Auth-Refresh-Token: <refresh_token>`</li><li>`X-Region: <region>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Review the [{{site.data.keyword.containerlong_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://containers.cloud.ibm.com/global/swagger-global-api) to find a list of supported APIs.

<br />


## Working with your cluster by using the Kubernetes API
{: #kube_api}

You can use the [Kubernetes API ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/using-api/api-overview/) to interact with your cluster in {{site.data.keyword.containerlong_notm}}.
{: shortdesc}

The following instructions require public network access in your cluster to connect to the public service endpoint of your Kubernetes master.
{: note}

1. Follow the steps in [Automating cluster deployments with the API](#cs_api) to retrieve your {{site.data.keyword.cloud_notm}} IAM access token, refresh token, the ID of the cluster where you want to run Kubernetes API requests, and the {{site.data.keyword.containerlong_notm}} region where your cluster is located.

2. Retrieve an {{site.data.keyword.cloud_notm}} IAM delegated refresh token.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Input parameters to get an IAM delegated refresh token with the input parameter in column 1 and the value in column 2.">
   <caption>Input parameters to get an IAM delegated refresh token. </caption>
   <thead>
   <th>Input parameters</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`</br><strong>Note</strong>: <code>Yng6Yng=</code> equals the URL-encoded authorization for the user name <strong>bx</strong> and the password <strong>bx</strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Body</td>
   <td><ul><li>`delegated_refresh_token_expiry: 600`</li>
   <li>`receiver_client_ids: kube`</li>
   <li>`response_type: delegated_refresh_token` </li>
   <li>`refresh_token`: Your {{site.data.keyword.cloud_notm}} IAM refresh token. </li>
   <li>`grant_type: refresh_token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Example output:
   ```
   {
    "delegated_refresh_token": <delegated_refresh_token>
   }
   ```
   {: screen}

3. Retrieve an {{site.data.keyword.cloud_notm}} IAM ID, IAM access, and IAM refresh token by using the delegated refresh token from the previous step. In your API output, you can find the IAM ID token in the **id_token** field, the IAM access token in the **access_token** field, and the IAM refresh token in the **refresh_token** field.
   ```
   POST https://iam.bluemix.net/identity/token
   ```
   {: codeblock}

   <table summary="Input parameters to get IAM ID and IAM access tokens with the input parameter in column 1 and the value in column 2.">
   <caption>Input parameters to get IAM ID and IAM access tokens.</caption>
   <thead>
   <th>Input parameters</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li> <li>`Authorization: Basic a3ViZTprdWJl`</br><strong>Note</strong>: <code>a3ViZTprdWJl</code> equals the URL-encoded authorization for the user name <strong><code>kube</code></strong> and the password <strong><code>kube</code></strong>.</li><li>`cache-control: no-cache`</li></ul>
   </td>
   </tr>
   <tr>
   <td>Body</td>
   <td><ul><li>`refresh_token`: Your {{site.data.keyword.cloud_notm}} IAM delegated refresh token. </li>
   <li>`grant_type: urn:ibm:params:oauth:grant-type:delegated-refresh-token`</li></ul></td>
   </tr>
   </tbody>
   </table>

   Example output:
   ```
   {
    "access_token": "<iam_access_token>",
    "id_token": "<iam_id_token>",
    "refresh_token": "<iam_refresh_token>",
    "token_type": "Bearer",
    "expires_in": 3600,
    "expiration": 1553629664,
    "scope": "ibm openid containers-kubernetes"
   }
   ```
   {: screen}

4. Retrieve the public URL of your Kubernetes master by using the IAM access token, the IAM ID token, the IAM refresh token and the {{site.data.keyword.containerlong_notm}} region that your cluster is in. You can find the URL in the **`publicServiceEndpointURL`** of your API output.
   ```
   GET https://containers.cloud.ibm.com/v1/clusters/<cluster_ID>
   ```
   {: codeblock}

   <table summary="Input parameters to get the public service endpoint for your Kubernetes master with the input parameter in column 1 and the value in column 2.">
   <caption>Input parameters to get the public service endpoint for your Kubernetes master.</caption>
   <thead>
   <th>Input parameters</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
     <td><ul><li>`Authorization`: Your {{site.data.keyword.cloud_notm}} IAM access token.</li><li>`X-Auth-Refresh-Token`: Your {{site.data.keyword.cloud_notm}} IAM refresh token.</li><li>`X-Region`: The {{site.data.keyword.containerlong_notm}} region of your cluster that you retrieved with the `GET https://containers.cloud.ibm.com/v1/clusters` API in [Automating cluster deployments with the API](#cs_api). </li></ul>
   </td>
   </tr>
   <tr>
   <td>Path</td>
   <td>`<cluster_ID>:` The ID of your cluster that you retrieved with the `GET https://containers.cloud.ibm.com/v1/clusters` API in [Automating cluster deployments with the API](#cs_api).      </td>
   </tr>
   </tbody>
   </table>

   Example output:
   ```
   {
    "location": "Dallas",
    "dataCenter": "dal10",
    "multiAzCapable": true,
    "vlans": null,
    "worker_vlans": null,
    "workerZones": [
        "dal10"
    ],
    "id": "1abc123b123b124ab1234a1a12a12a12",
    "name": "mycluster",
    "region": "us-south",
    ...
    "publicServiceEndpointURL": "https://c7.us-south.containers.cloud.ibm.com:27078"
   }
   ```
   {: screen}

5. Run Kubernetes API requests against your cluster by using the IAM ID token that you retrieved earlier. For example, list the Kubernetes version that runs in your cluster.

   If you enabled SSL certificate verification in your API test framework, make sure to disable this feature.
   {: tip}

   ```
   GET <publicServiceEndpointURL>/version
   ```
   {: codeblock}

   <table summary="Input parameters to view the Kubernetes version that runs in your cluster with the input parameter in column 1 and the value in column 2. ">
   <caption>Input parameters to view the Kubernetes version that runs in your cluster. </caption>
   <thead>
   <th>Input parameters</th>
   <th>Values</th>
   </thead>
   <tbody>
   <tr>
   <td>Header</td>
   <td>`Authorization: bearer <id_token>`</td>
   </tr>
   <tr>
   <td>Path</td>
   <td>`<publicServiceEndpointURL>`: The **`publicServiceEndpointURL`** of your Kubernetes master that you retrieved in the previous step.      </td>
   </tr>
   </tbody>
   </table>

   Example output:
   ```
   {
    "major": "1",
    "minor": "13",
    "gitVersion": "v1.13.4+IKS",
    "gitCommit": "c35166bd86eaa91d17af1c08289ffeab3e71e11e",
    "gitTreeState": "clean",
    "buildDate": "2019-03-21T10:08:03Z",
    "goVersion": "go1.11.5",
    "compiler": "gc",
    "platform": "linux/amd64"
   }
   ```
   {: screen}

6. Review the [Kubernetes API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/reference/kubernetes-api/) to find a list of supported APIs for the latest Kubernetes version. Make sure to use the API documentation that matches the Kubernetes version of your cluster. If you do not use the latest Kubernetes version, append your version at the end of the URL. For example, to access the API documentation for version 1.12, add `v1.12`.


## Refreshing {{site.data.keyword.cloud_notm}} IAM access tokens and obtaining new refresh tokens with the API
{: #cs_api_refresh}

Every {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) access token that is issued via the API expires after one hour. You must refresh your access token on a regular basis to assure access to the {{site.data.keyword.cloud_notm}} API. You can use the same steps to obtain a new refresh token.
{:shortdesc}

Before you begin, make sure that you have an {{site.data.keyword.cloud_notm}} IAM refresh token or an {{site.data.keyword.cloud_notm}} API key that you can use to request a new access token.
- **Refresh token:** Follow the instructions in [Automating the cluster creation and management process with the {{site.data.keyword.cloud_notm}} API](#cs_api).
- **API key:** Retrieve your [{{site.data.keyword.cloud_notm}} ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/) API key as follows.
   1. From the menu bar, click **Manage** > **Access (IAM)**.
   2. Click the **Users** page and then select yourself.
   3. In the **API keys** pane, click **Create an IBM Cloud API key**.
   4. Enter a **Name** and **Description** for your API key and click **Create**.
   4. Click **Show** to see the API key that was generated for you.
   5. Copy the API key so that you can use it to retrieve your new {{site.data.keyword.cloud_notm}} IAM access token.

Use the following steps if you want to create an {{site.data.keyword.cloud_notm}} IAM token or if you want to obtain a new refresh token.

1.  Generate a new {{site.data.keyword.cloud_notm}} IAM access token by using the refresh token or the {{site.data.keyword.cloud_notm}} API key.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Input parameters for new IAM token with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters for a new {{site.data.keyword.cloud_notm}} IAM token</caption>
    <thead>
    <th>Input parameters</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>Note:</strong> <code>Yng6Yng=</code> equals the URL-encoded authorization for the user name <strong>bx</strong> and the password <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Body when using the refresh token</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` Your {{site.data.keyword.cloud_notm}} IAM refresh token. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` Your {{site.data.keyword.cloud_notm}} account ID. </li></ul><strong>Note</strong>: Add the <code>uaa_client_secret</code> key with no value specified.</td>
    </tr>
    <tr>
      <td>Body when using the {{site.data.keyword.cloud_notm}} API key</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` Your {{site.data.keyword.cloud_notm}} API key. </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>Note:</strong> Add the <code>uaa_client_secret</code> key with no value specified.</td>
    </tr>
    </tbody>
    </table>

    Example API output:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    You can find your new {{site.data.keyword.cloud_notm}} IAM token in the **access_token**, and the refresh token in the **refresh_token** field of your API output.

2.  Continue working with the [{{site.data.keyword.containerlong_notm}} API documentation ![External link icon](../icons/launch-glyph.svg "External link icon")](https://containers.cloud.ibm.com/global/swagger-global-api) by using the token from the previous step.

<br />


## Refreshing {{site.data.keyword.cloud_notm}} IAM access tokens and obtaining new refresh tokens with the CLI
{: #cs_cli_refresh}

When you start a new CLI session, or if 24 hours has expired in your current CLI session, you must set the context for your cluster by running `ibmcloud ks cluster-config --cluster <cluster_name>`. When you set the context for your cluster with this command, the `kubeconfig` file for your Kubernetes cluster is downloaded. Additionally, an {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) ID token and a refresh token are issued to provide authentication.
{: shortdesc}

**ID token**: Every IAM ID token that is issued via the CLI expires after one hour. When the ID token expires, the refresh token is sent to the token provider to refresh the ID token. Your authentication is refreshed, and you can continue to run commands against your cluster.

**Refresh token**: Refresh tokens expire every 30 days. If the refresh token is expired, the ID token cannot be refreshed, and you are not able to continue running commands in the CLI. You can get a new refresh token by running `ibmcloud ks cluster-config --cluster <cluster_name>`. This command also refreshes your ID token.



