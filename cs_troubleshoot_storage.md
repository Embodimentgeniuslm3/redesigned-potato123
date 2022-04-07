---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-19"

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
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Troubleshooting cluster storage
{: #cs_troubleshoot_storage}

As you use {{site.data.keyword.containerlong}}, consider these techniques for troubleshooting cluster storage.
{: shortdesc}

If you have a more general issue, try out [cluster debugging](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}


## Debugging persistent storage failures
{: #debug_storage}

Review the options to debug persistent storage and find the root causes for failures.
{: shortdesc}

1. Verify that you use the latest {{site.data.keyword.cloud_notm}} and {{site.data.keyword.containerlong_notm}} plug-in version.
   ```
   ibmcloud update
   ```
   {: pre}

   ```
   ibmcloud plugin repo-plugins
   ```
   {: pre}

2. Verify that the `kubectl` CLI version that you run on your local machine matches the Kubernetes version that is installed in your cluster.
   1. Show the `kubectl` CLI version that is installed in your cluster and your local machine.
      ```
      kubectl version
      ```
      {: pre}

      Example output:
      ```
      Client Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5", GitCommit:"641856db18352033a0d96dbc99153fa3b27298e5", GitTreeState:"clean", BuildDate:"2019-03-25T15:53:57Z", GoVersion:"go1.12.1", Compiler:"gc", Platform:"darwin/amd64"}
      Server Version: version.Info{Major:"1", Minor:"13", GitVersion:"v1.13.5+IKS", GitCommit:"e15454c2216a73b59e9a059fd2def4e6712a7cf0", GitTreeState:"clean", BuildDate:"2019-04-01T10:08:07Z", GoVersion:"go1.11.5", Compiler:"gc", Platform:"linux/amd64"}
      ```
      {: screen}

      The CLI versions match if you can see the same version in `GitVersion` for the client and the server. You can ignore the `+IKS` part of the version for the server.
   2. If the `kubectl` CLI versions on your local machine and your cluster do not match, either [update your cluster](/docs/containers?topic=containers-update) or [install a different CLI version on your local machine](/docs/containers?topic=containers-cs_cli_install#kubectl).

3. For block storage, object storage, and Portworx only: Make sure that you [installed the Helm server Tiller with a Kubernetes services account](/docs/containers?topic=containers-helm#public_helm_install).

4. For block storage, object storage, and Portworx only: Make sure that you installed the latest Helm chart version for the plug-in.

   **Block and object storage**:

   1. Update your Helm chart repositories.
      ```
      helm repo update
      ```
      {: pre}

   2. List the Helm charts in the `iks-charts` repository.
      ```
      helm search iks-charts
      ```
      {: pre}

      Example output:
      ```
      iks-charts/ibm-block-storage-attacher          	1.0.2        A Helm chart for installing ibmcloud block storage attach...
      iks-charts/ibm-iks-cluster-autoscaler          	1.0.5        A Helm chart for installing the IBM Cloud cluster autoscaler
      iks-charts/ibm-object-storage-plugin           	1.0.6        A Helm chart for installing ibmcloud object storage plugin  
      iks-charts/ibm-worker-recovery                 	1.10.46      IBM Autorecovery system allows automatic recovery of unhe...
      ...
      ```
      {: screen}

   3. List the installed Helm charts in your cluster and compare the version that you installed with the version that is available.
      ```
      helm ls
      ```
      {: pre}

   4. If a more recent version is available, install this version. For instructions, see [Updating the {{site.data.keyword.cloud_notm}} Block Storage plug-in](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in) and [Updating the {{site.data.keyword.cos_full_notm}} plug-in](/docs/containers?topic=containers-object_storage#update_cos_plugin).

   **Portworx**:

   1. Find the [latest Helm chart version ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/portworx/helm/tree/master/charts/portworx) that is available.

   2. List the installed Helm charts in your cluster and compare the version that you installed with the version that is available.
      ```
      helm ls
      ```
      {: pre}

   3. If a more recent version is available, install this version. For instructions, see [Updating Portworx in your cluster](/docs/containers?topic=containers-portworx#update_portworx).

5. Verify that the storage driver and plug-in pods show a status of **Running**.
   1. List the pods in the `kube-system` namespace.
      ```
      kubectl get pods -n kube-system
      ```
      {: pre}

   2. If the pods do not show a **Running** status, get more details of the pod to find the root cause. Depending on the status of your pod, you might not be able to execute all of the following commands.
      ```
      kubectl describe pod <pod_name> -n kube-system
      ```
      {: pre}

      ```
      kubectl logs <pod_name> -n kube-system
      ```
      {: pre}

   3. Analyze the **Events** section of the CLI output of the `kubectl describe pod` command and the latest logs to find the root cause for the error.

6. Check whether your PVC is successfully provisioned.
   1. Check the state of your PVC. A PVC is successfully provisioned if the PVC shows a status of **Bound**.
      ```
      kubectl get pvc
      ```
      {: pre}

   2. If the state of the PVC shows **Pending**, retrieve the error for why the PVC remains pending.
      ```
      kubectl describe pvc <pvc_name>
      ```
      {: pre}

   3. Review common errors that can occur during the PVC creation.
      - [File storage and block storage: PVC remains in a pending state](#file_pvc_pending)
      - [Object storage: PVC remains in a pending state](#cos_pvc_pending)

7. Check whether the pod that mounts your storage instance is successfully deployed.
   1. List the pods in your cluster. A pod is successfully deployed if the pod shows a status of **Running**.
      ```
      kubectl get pods
      ```
      {: pre}

   2. Get the details of your pod and check whether errors are displayed in the **Events** section of your CLI output.
      ```
      kubectl describe pod <pod_name>
      ```
      {: pre}

   3. Retrieve the logs for your app and check whether you can see any error messages.
      ```
      kubectl logs <pod_name>
      ```
      {: pre}

   4. Review common errors that can occur when you mount a PVC to your app.
      - [File storage: App cannot access or write to PVC](#file_app_failures)
      - [Block storage: App cannot access or write to PVC](#block_app_failures)
      - [Object storage: Accessing files with a non-root user fails](#cos_nonroot_access)


## File storage and block storage: PVC remains in a pending state
{: #file_pvc_pending}

{: tsSymptoms}
When you create a PVC and you run `kubectl get pvc <pvc_name>`, your PVC remains in a **Pending** state, even after waiting for some time.

{: tsCauses}
During the PVC creation and binding, many different tasks are executed by the file and block storage plug-in. Each task can fail and cause a different error message.

{: tsResolve}

1. Find the root cause for why the PVC remains in a **Pending** state.
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}

2. Review common error message descriptions and resolutions.

   <table>
   <thead>
     <th>Error message</th>
     <th>Description</th>
     <th>Steps to resolve</th>
  </thead>
  <tbody>
    <tr>
      <td><code>User doesn't have permissions to create or manage Storage</code></br></br><code>Failed to find any valid softlayer credentials in configuration file</code></br></br><code>Storage with the order ID %d could not be created after retrying for %d seconds.</code></br></br><code>Unable to locate datacenter with name <datacenter_name>.</code></td>
      <td>The IAM API key or the IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster does not have all the required permissions to provision persistent storage. </td>
      <td>See [PVC creation fails because of missing permissions](#missing_permissions). </td>
    </tr>
    <tr>
      <td><code>Your order will exceed the maximum number of storage volumes allowed. Please contact Sales</code></td>
      <td>Every {{site.data.keyword.cloud_notm}} account is set up with a maximum number of storage instances that can be created. By creating the PVC, you exceed the maximum number of storage instances. </td>
      <td>To create a PVC, choose from the following options. <ul><li>Remove any unused PVCs.</li><li>Ask the {{site.data.keyword.cloud_notm}} account owner to increase your storage quota by [opening a support case](/docs/get-support?topic=get-support-getting-customer-support).</li></ul> </td>
    </tr>
    <tr>
      <td><code>Unable to find the exact ItemPriceIds(type|size|iops) for the specified storage</code> </br></br><code>Failed to place storage order with the storage provider</code></td>
      <td>The storage size and IOPS that you specified in your PVC are not supported by the storage type that you chose and cannot be used with the specified storage class. </td>
      <td>Review [Deciding on the file storage configuration](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) and [Deciding on the block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to find supported storage sizes and IOPS for the storage class that you want to use. Correct the size and IOPS, and recreate the PVC. </td>
    </tr>
    <tr>
  <td><code>Failed to find the datacenter name in configuration file</code></td>
      <td>The data center that you specified in your PVC does not exist. </td>
  <td>Run <code>ibmcloud ks locations</code> to list available data centers. Correct the data center in your PVC and recreate the PVC.</td>
    </tr>
    <tr>
  <td><code>Failed to place storage order with the storage provider</code></br></br><code>Storage with the order ID 12345 could not be created after retrying for xx seconds. </code></br></br><code>Failed to do subnet authorizations for the storage 12345.</code><code>Storage 12345 has ongoing active transactions and could not be completed after retrying for xx seconds.</code></td>
  <td>The storage size, IOPS, and storage type might be incompatible with the storage class that you chose, or the {{site.data.keyword.cloud_notm}} infrastructure API endpoint is currently unavailable. </td>
  <td>Review [Deciding on the file storage configuration](/docs/containers?topic=containers-file_storage#file_predefined_storageclass) and [Deciding on the block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to find supported storage sizes and IOPS for the storage class and storage type that you want to use. Then, delete the PVC and recreate the PVC. </td>
  </tr>
  <tr>
  <td><code>Failed to find the storage with storage id 12345. </code></td>
  <td>You want to create a PVC for an existing storage instance by using Kubernetes static provisioning, but the storage instance that you specified could not be found. </td>
  <td>Follow the instructions to provision existing [file storage](/docs/containers?topic=containers-file_storage#existing_file) or [block storage](/docs/containers?topic=containers-block_storage#existing_block) in your cluster and make sure to retrieve the correct information for your existing storage instance. Then, delete the PVC and recreate the PVC.  </td>
  </tr>
  <tr>
  <td><code>Storage type not provided, expected storage type is `Endurance` or `Performance`. </code></td>
  <td>You created a custom storage class and specified a storage type that is not supported.</td>
  <td>Update your custom storage class to specify `Endurance` or `Performance` as your storage type. To find examples for custom storage classes, see the sample custom storage classes for [file storage](/docs/containers?topic=containers-file_storage#file_custom_storageclass) and [block storage](/docs/containers?topic=containers-block_storage#block_custom_storageclass). </td>
  </tr>
  </tbody>
  </table>

## File storage: App cannot access or write to PVC
{: #file_app_failures}

When you mount a PVC to your pod, you might experience errors when accessing or writing to the PVC.
{: shortdesc}

1. List the pods in your cluster and review the status of the pod.
   ```
   kubectl get pods
   ```
   {: pre}

2. Find the root cause for why your app cannot access or write to the PVC.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}

   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Review common errors that can occur when you mount a PVC to a pod.
   <table>
   <thead>
     <th>Symptom or error message</th>
     <th>Description</th>
     <th>Steps to resolve</th>
  </thead>
  <tbody>
    <tr>
      <td>Your pod is stuck in a <strong>ContainerCreating</strong> state. </br></br><code>MountVolume.SetUp failed for volume ... read-only file system</code></td>
      <td>The {{site.data.keyword.cloud_notm}} infrastructure back end experienced network problems. To protect your data and to avoid data corruption, {{site.data.keyword.cloud_notm}} automatically disconnected the file storage server to prevent write operations on NFS file shares.  </td>
      <td>See [File storage: File systems for worker nodes change to read-only](#readonly_nodes)</td>
      </tr>
      <tr>
  <td><code>write-permission</code> </br></br><code>do not have required permission</code></br></br><code>cannot create directory '/bitnami/mariadb/data': Permission denied </code></td>
  <td>In your deployment, you specified a non-root user to own the NFS file storage mount path. By default, non-root users do not have write permission on the volume mount path for NFS-backed storage. </td>
  <td>See [File storage: App fails when a non-root user owns the NFS file storage mount path](#nonroot)</td>
  </tr>
  <tr>
  <td>After you specified a non-root user to own the NFS file storage mount path or deployed a Helm chart with a non-root user ID specified, the user cannot write to the mounted storage.</td>
  <td>The deployment or Helm chart configuration specifies the security context for the pod's <code>fsGroup</code> (group ID) and <code>runAsUser</code> (user ID)</td>
  <td>See [File storage: Adding non-root user access to persistent storage fails](#cs_storage_nonroot)</td>
  </tr>
</tbody>
</table>

### File storage: File systems for worker nodes change to read-only
{: #readonly_nodes}

{: tsSymptoms}
{: #stuck_creating_state}
You might see one of the following symptoms:
- When you run `kubectl get pods -o wide`, you see that multiple pods that are running on the same worker node are stuck in the `ContainerCreating` state.
- When you run a `kubectl describe` command, you see the following error in the **Events** section: `MountVolume.SetUp failed for volume ... read-only file system`.

{: tsCauses}
The file system on the worker node is read-only.

{: tsResolve}
1.  Back up any data that might be stored on the worker node or in your containers.
2.  For a short-term fix to the existing worker node, reload the worker node.
    <pre class="pre"><code>ibmcloud ks worker-reload --cluster &lt;cluster_name&gt; --worker &lt;worker_ID&gt;</code></pre>

For a long-term fix, [update the machine type of your worker pool](/docs/containers?topic=containers-update#machine_type).

<br />



### File storage: App fails when a non-root user owns the NFS file storage mount path
{: #nonroot}

{: tsSymptoms}
After you [add NFS storage](/docs/containers?topic=containers-file_storage#file_app_volume_mount) to your deployment, the deployment of your container fails. When you retrieve the logs for your container, you might see errors such as the following. The pod fails and is stuck in a reload cycle.

```
write-permission
```
{: screen}

```
do not have required permission
```
{: screen}

```
cannot create directory '/bitnami/mariadb/data': Permission denied
```
{: screen}

{: tsCauses}
By default, non-root users do not have write permission on the volume mount path for NFS-backed storage. Some common app images, such as Jenkins and Nexus3, specify a non-root user that owns the mount path in the Dockerfile. When you create a container from this Dockerfile, the creation of the container fails due to insufficient permissions of the non-root user on the mount path. To grant write permission, you can modify the Dockerfile to temporarily add the non-root user to the root user group before it changes the mount path permissions, or use an init container.

If you use a Helm chart to deploy the image, edit the Helm deployment to use an init container.
{:tip}



{: tsResolve}
When you include an [init container![External link icon](../icons/launch-glyph.svg "External link icon")](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) in your deployment, you can give a non-root user that is specified in your Dockerfile write permissions for the volume mount path inside the container. The init container starts before your app container starts. The init container creates the volume mount path inside the container, changes the mount path to be owned by the correct (non-root) user, and closes. Then, your app container starts with the non-root user that must write to the mount path. Because the path is already owned by the non-root user, writing to the mount path is successful. If you do not want to use an init container, you can modify the Dockerfile to add non-root user access to NFS file storage.


Before you begin: [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

1.  Open the Dockerfile for your app and get the user ID (UID) and group ID (GID) from the user that you want to give writer permission on the volume mount path. In the example from a Jenkins Dockerfile, the information is:
    - UID: `1000`
    - GID: `1000`

    **Example**:

    ```
    FROM openjdk:8-jdk

    RUN apt-get update && apt-get install -y git curl && rm -rf /var/lib/apt/lists/*

    ARG user=jenkins
    ARG group=jenkins
    ARG uid=1000
    ARG gid=1000
    ARG http_port=8080
    ARG agent_port=50000

    ENV JENKINS_HOME /var/jenkins_home
    ENV JENKINS_SLAVE_AGENT_PORT ${agent_port}
    ...
    ```
    {:screen}

2.  Add persistent storage to your app by creating a persistent volume claim (PVC). This example uses the `ibmc-file-bronze` storage class. To review available storage classes, run `kubectl get storageclasses`.

    ```
    apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: mypvc
      annotations:
        volume.beta.kubernetes.io/storage-class: "ibmc-file-bronze"
    spec:
      accessModes:
        - ReadWriteMany
      resources:
        requests:
          storage: 20Gi
    ```
    {: codeblock}

3.  Create the PVC.

    ```
    kubectl apply -f mypvc.yaml
    ```
    {: pre}

4.  In your deployment `.yaml` file, add the init container. Include the UID and GID that you previously retrieved.

    ```
    initContainers:
    - name: initcontainer # Or replace the name
      image: alpine:latest
      command: ["/bin/sh", "-c"]
      args:
        - chown <UID>:<GID> /mount; # Replace UID and GID with values from the Dockerfile
      volumeMounts:
      - name: volume # Or you can replace with any name
        mountPath: /mount # Must match the mount path in the args line
    ```
    {: codeblock}

    **Example** for a Jenkins deployment:

    ```
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: my_pod
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: jenkins      
      template:
        metadata:
          labels:
            app: jenkins
        spec:
          containers:
          - name: jenkins
            image: jenkins
            volumeMounts:
            - mountPath: /var/jenkins_home
              name: volume
          volumes:
          - name: volume
            persistentVolumeClaim:
              claimName: mypvc
          initContainers:
          - name: permissionsfix
            image: alpine:latest
            command: ["/bin/sh", "-c"]
            args:
              - chown 1000:1000 /mount;
            volumeMounts:
            - name: volume
              mountPath: /mount
    ```
    {: codeblock}

5.  Create the pod and mount the PVC to your pod.

    ```
    kubectl apply -f my_pod.yaml
    ```
    {: pre}

6. Verify that the volume is successfully mounted to your pod. Note the pod name and **Containers/Mounts** path.

    ```
    kubectl describe pod <my_pod>
    ```
    {: pre}

    **Example output**:

    ```
    Name:       mypod-123456789
    Namespace:	default
    ...
    Init Containers:
    ...
    Mounts:
      /mount from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Containers:
      jenkins:
        Container ID:
        Image:		jenkins
        Image ID:
        Port:		  <none>
        State:		Waiting
          Reason:		PodInitializing
        Ready:		False
        Restart Count:	0
        Environment:	<none>
        Mounts:
          /var/jenkins_home from volume (rw)
          /var/run/secrets/kubernetes.io/serviceaccount from default-token-cp9f0 (ro)
    ...
    Volumes:
      myvol:
        Type:	PersistentVolumeClaim (a reference to a PersistentVolumeClaim in the same namespace)
        ClaimName:	mypvc
        ReadOnly:	  false

    ```
    {: screen}

7.  Log in to the pod by using the pod name that you previously noted.

    ```
    kubectl exec -it <my_pod-123456789> /bin/bash
    ```
    {: pre}

8. Verify the permissions of your container's mount path. In the example, the mount path is `/var/jenkins_home`.

    ```
    ls -ln /var/jenkins_home
    ```
    {: pre}

    **Example output**:

    ```
    jenkins@mypod-123456789:/$ ls -ln /var/jenkins_home
    total 12
    -rw-r--r-- 1 1000 1000  102 Mar  9 19:58 copy_reference_file.log
    drwxr-xr-x 2 1000 1000 4096 Mar  9 19:58 init.groovy.d
    drwxr-xr-x 9 1000 1000 4096 Mar  9 20:16 war
    ```
    {: screen}

    This output shows that the GID and UID from your Dockerfile (in this example, `1000` and `1000`) own the mount path inside the container.

<br />


### File storage: Adding non-root user access to persistent storage fails
{: #cs_storage_nonroot}

{: tsSymptoms}
After you [add non-root user access to persistent storage](#nonroot) or deploy a Helm chart with a non-root user ID specified, the user cannot write to the mounted storage.

{: tsCauses}
The deployment or Helm chart configuration specifies the [security context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) for the pod's `fsGroup` (group ID) and `runAsUser` (user ID). Currently, {{site.data.keyword.containerlong_notm}} does not support the `fsGroup` specification, and supports only `runAsUser` set as `0` (root permissions).

{: tsResolve}
Remove the configuration's `securityContext` fields for `fsGroup` and `runAsUser` from the image, deployment, or Helm chart configuration file and redeploy. If you need to change the ownership of the mount path from `nobody`, [add non-root user access](#nonroot). After you add the [non-root `initContainer`](#nonroot), set `runAsUser` at the container level, not the pod level.

<br />




## Block storage: App cannot access or write to PVC
{: #block_app_failures}

When you mount a PVC to your pod, you might experience errors when accessing or writing to the PVC.
{: shortdesc}

1. List the pods in your cluster and review the status of the pod.
   ```
   kubectl get pods
   ```
   {: pre}

2. Find the root cause for why your app cannot access or write to the PVC.
   ```
   kubectl describe pod <pod_name>
   ```
   {: pre}

   ```
   kubectl logs <pod_name>
   ```
   {: pre}

3. Review common errors that can occur when you mount a PVC to a pod.
   <table>
   <thead>
     <th>Symptom or error message</th>
     <th>Description</th>
     <th>Steps to resolve</th>
  </thead>
  <tbody>
    <tr>
      <td>Your pod is stuck in a <strong>ContainerCreating</strong> or <strong>CrashLoopBackOff</strong> state. </br></br><code>MountVolume.SetUp failed for volume ... read-only.</code></td>
      <td>The {{site.data.keyword.cloud_notm}} infrastructure back end experienced network problems. To protect your data and to avoid data corruption, {{site.data.keyword.cloud_notm}} automatically disconnected the block storage server to prevent write operations on block storage instances.  </td>
      <td>See [Block storage: Block storage changes to read-only](#readonly_block)</td>
      </tr>
      <tr>
  <td><code>failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32</code> </td>
        <td>You want to mount an existing block storage instance that was set up with an <code>XFS</code> file system. When you created the PV and the matching PVC, you specified an <code>ext4</code> or no file system. The file system that you specify in your PV must be the same file system that is set up in your block storage instance. </td>
  <td>See [Block storage: Mounting existing block storage to a pod fails due to the wrong file system](#block_filesystem)</td>
  </tr>
</tbody>
</table>

### Block storage: Block storage changes to read-only
{: #readonly_block}

{: tsSymptoms}
You might see the following symptoms:
- When you run `kubectl get pods -o wide`, you see that multiple pods on the same worker node are stuck in the `ContainerCreating` or `CrashLoopBackOff` state. All these pods use the same block storage instance.
- When you run a `kubectl describe pod` command, you see the following error in the **Events** section: `MountVolume.SetUp failed for volume ... read-only`.

{: tsCauses}
If a network error occurs while a pod writes to a volume, IBM Cloud infrastructure protects the data on the volume from getting corrupted by changing the volume to a read-only mode. Pods that use this volume cannot continue to write to the volume and fail.

{: tsResolve}
1. Check the version of the {{site.data.keyword.cloud_notm}} Block Storage plug-in that is installed in your cluster.
   ```
   helm ls
   ```
   {: pre}

2. Verify that you use the [latest version of the {{site.data.keyword.cloud_notm}} Block Storage plug-in](https://cloud.ibm.com/kubernetes/solutions/helm-charts/ibm/ibmcloud-block-storage-plugin). If not, [update your plug-in](/docs/containers?topic=containers-block_storage#updating-the-ibm-cloud-block-storage-plug-in).
3. If you used a Kubernetes deployment for your pod, restart the pod that is failing by removing the pod and letting Kubernetes re-create it. If you did not use a deployment, retrieve the YAML file that was used to create your pod by running `kubectl get pod <pod_name> -o yaml >pod.yaml`. Then, delete and manually re-create the pod.
    ```
    kubectl delete pod <pod_name>
    ```
    {: pre}

4. Check whether re-creating your pod resolved the issue. If not, reload the worker node.
   1. Find the worker node where your pod runs and note the private IP address that is assigned to your worker node.
      ```
      kubectl describe pod <pod_name> | grep Node
      ```
      {: pre}

      Example output:
      ```
      Node:               10.75.XX.XXX/10.75.XX.XXX
      Node-Selectors:  <none>
      ```
      {: screen}

   2. Retrieve the **ID** of your worker node by using the private IP address from the previous step.
      ```
      ibmcloud ks workers --cluster <cluster_name_or_ID>
      ```
      {: pre}

   3. Gracefully [reload the worker node](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_worker_reload).


<br />


### Block storage: Mounting existing block storage to a pod fails due to the wrong file system
{: #block_filesystem}

{: tsSymptoms}
When you run `kubectl describe pod <pod_name>`, you see the following error:
```
failed to mount the volume as "ext4", it already contains xfs. Mount error: mount failed: exit status 32
```
{: screen}

{: tsCauses}
You have an existing block storage device that is set up with an `XFS` file system. To mount this device to your pod, you [created a PV](/docs/containers?topic=containers-block_storage#existing_block) that specified `ext4` as your file system or no file system in the `spec/flexVolume/fsType` section. If no file system is defined, the PV defaults to `ext4`.
The PV was created successfully and was linked to your existing block storage instance. However, when you try to mount the PV to your cluster by using a matching PVC, the volume fails to mount. You cannot mount your `XFS` block storage instance with an `ext4` file system to the pod.

{: tsResolve}
Update the file system in the existing PV from `ext4` to `XFS`.

1. List the existing PVs in your cluster and note the name of the PV that you used for your existing block storage instance.
   ```
   kubectl get pv
   ```
   {: pre}

2. Save the PV YAML on your local machine.
   ```
   kubectl get pv <pv_name> -o yaml > <filepath/xfs_pv.yaml>
   ```
   {: pre}

3. Open the YAML file and change the `fsType` from `ext4` to `xfs`.
4. Replace the PV in your cluster.
   ```
   kubectl replace --force -f <filepath/xfs_pv.yaml>
   ```
   {: pre}

5. Log in to the pod where you mounted the PV.
   ```
   kubectl exec -it <pod_name> sh
   ```
   {: pre}

6. Verify that the file system changed to `XFS`.
   ```
   df -Th
   ```
   {: pre}

   Example output:
   ```
   Filesystem Type Size Used Avail Use% Mounted on /dev/mapper/3600a098031234546d5d4c9876654e35 xfs 20G 33M 20G 1% /myvolumepath
   ```
   {: screen}

<br />



## Object storage: Installing the {{site.data.keyword.cos_full_notm}} `ibmc` Helm plug-in fails
{: #cos_helm_fails}

{: tsSymptoms}
When you install the {{site.data.keyword.cos_full_notm}} `ibmc` Helm plug-in, the installation fails with one of the following errors:
```
Error: symlink /Users/iks-charts/ibm-object-storage-plugin/helm-ibmc /Users/ibm/.helm/plugins/helm-ibmc: file exists
```
{: screen}

```
Error: fork/exec /home/iksadmin/.helm/plugins/helm-ibmc/ibmc.sh: permission denied
```
{: screen}

{: tsCauses}
When the `ibmc` Helm plug-in is installed, a symlink is created from the `./helm/plugins/helm-ibmc` directory to the directory where the `ibmc` Helm plug-in is located on your local system, which is usually in `./ibmcloud-object-storage-plugin/helm-ibmc`. When you remove the `ibmc` Helm plug-in from your local system, or you move the `ibmc` Helm plug-in directory to a different location, the symlink is not removed.

If you see a `permission denied` error, you do not have the required `read`, `write`, and `execute` permission on the `ibmc.sh` bash file so that you can execute `ibmc` Helm plug-in commands.

{: tsResolve}

**For symlink errors**:

1. Remove the {{site.data.keyword.cos_full_notm}} Helm plug-in.
   ```
   rm -rf ~/.helm/plugins/helm-ibmc
   ```
   {: pre}

2. Follow the [documentation](/docs/containers?topic=containers-object_storage#install_cos) to re-install the `ibmc` Helm plug-in and the {{site.data.keyword.cos_full_notm}} plug-in.

**For permission errors**:

1. Change the permissions for the `ibmc` plug-in.
   ```
   chmod 755 ~/.helm/plugins/helm-ibmc/ibmc.sh
   ```
   {: pre}

2. Try out the `ibm` Helm plug-in.
   ```
   helm ibmc --help
   ```
   {: pre}

3. [Continue installing the {{site.data.keyword.cos_full_notm}} plug-in](/docs/containers?topic=containers-object_storage#install_cos).


<br />


## Object storage: PVC remains in a pending state
{: #cos_pvc_pending}

{: tsSymptoms}
When you create a PVC and you run `kubectl get pvc <pvc_name>`, your PVC remains in a **Pending** state, even after waiting for some time.

{: tsCauses}
During the PVC creation and binding, many different tasks are executed by the {{site.data.keyword.cos_full_notm}} plug-in. Each task can fail and cause a different error message.

{: tsResolve}

1. Find the root cause for why the PVC remains in a **Pending** state.
   ```
   kubectl describe pvc <pvc_name> -n <namespace>
   ```
   {: pre}

2. Review common error message descriptions and resolutions.

   <table>
   <thead>
     <th>Error message</th>
     <th>Description</th>
     <th>Steps to resolve</th>
  </thead>
  <tbody>
    <tr>
      <td>`User doesn't have permissions to create or manage Storage`</td>
      <td>The IAM API key or the IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster does not have all the required permissions to provision persistent storage. </td>
      <td>See [PVC creation fails because of missing permissions](#missing_permissions). </td>
    </tr>
    <tr>
      <td>`cannot get credentials: cannot get secret <secret_name>: secrets "<secret_name>" not found`</td>
      <td>The Kubernetes secret that holds your {{site.data.keyword.cos_full_notm}} service credentials does not exist in the same namespace as the PVC or pod. </td>
      <td>See [PVC or pod creation fails due to not finding the Kubernetes secret](#cos_secret_access_fails).</td>
    </tr>
    <tr>
      <td>`cannot get credentials: Wrong Secret Type.Provided secret of type XXXX.Expected type ibm/ibmc-s3fs`</td>
      <td>The Kubernetes secret that you created for your {{site.data.keyword.cos_full_notm}} service instance does not include the `type: ibm/ibmc-s3fs`.</td>
      <td>Edit the Kubernetes secret that holds your {{site.data.keyword.cos_full_notm}} credentials to add or change the `type` to `ibm/ibmc-s3fs`. </td>
    </tr>
    <tr>
      <td>`Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>`</br> </br> `Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname< or https://<hostname>`</td>
      <td>The s3fs API or IAM API endpoint has the wrong format, or the s3fs API endpoint could not be retrieved based on your cluster location.  </td>
      <td>See [PVC creation fails due to wrong s3fs API endpoint](#cos_api_endpoint_failure).</td>
    </tr>
    <tr>
      <td>`object-path cannot be set when auto-create is enabled`</td>
      <td>You specified an existing subdirectory in your bucket that you want to mount to your PVC by using the `ibm.io/object-path` annotation. If you set a subdirectory, you must disable the bucket auto-create feature.  </td>
      <td>In your PVC, set `ibm.io/auto-create-bucket: "false"` and provide the name of the existing bucket in `ibm.io/bucket`.</td>
    </tr>
    <tr>
    <td>`bucket auto-create must be enabled when bucket auto-delete is enabled`</td>
    <td>In your PVC, you set `ibm.io/auto-delete-bucket: true` to automatically delete your data, the bucket, and the PV when you remove the PVC. This option requires `ibm.io/auto-create-bucket` to be set to <strong>true</strong>, and `ibm.io/bucket` to be set to `""` at the same time.</td>
    <td>In your PVC, set `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""` so that your bucket is automatically created with a name with the format `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`bucket cannot be set when auto-delete is enabled`</td>
    <td>In your PVC, you set `ibm.io/auto-delete-bucket: true` to automatically delete your data, the bucket, and the PV when you remove the PVC. This option requires `ibm.io/auto-create-bucket` to be set to <strong>true</strong>, and `ibm.io/bucket` to be set to `""` at the same time.</td>
    <td>In your PVC, set `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""` so that your bucket is automatically created with a name with the format `tmp-s3fs-xxxx`. </td>
    </tr>
    <tr>
    <td>`cannot create bucket using API key without service-instance-id`</td>
    <td>If you want to use IAM API keys to access your {{site.data.keyword.cos_full_notm}} service instance, you must store the API key and the ID of the {{site.data.keyword.cos_full_notm}} service instance in a Kubernetes secret.  </td>
    <td>See [Creating a secret for the object storage service credentials](/docs/containers?topic=containers-object_storage#create_cos_secret). </td>
    </tr>
    <tr>
      <td>`object-path “<subdirectory_name>” not found inside bucket <bucket_name>`</td>
      <td>You specified an existing subdirectory in your bucket that you want to mount to your PVC by using the `ibm.io/object-path` annotation. This subdirectory could not be found in the bucket that you specified. </td>
      <td>Verify that the subdirectory that you specified in `ibm.io/object-path` exists in the bucket that you specified in `ibm.io/bucket`. </td>
    </tr>
        <tr>
          <td>`BucketAlreadyExists: The requested bucket name is not available. The bucket namespace is shared by all users of the system. Please select a different name and try again.`</td>
          <td>You set `ibm.io/auto-create-bucket: true` and specified a bucket name at the same time, or you specified a bucket name that already exists in {{site.data.keyword.cos_full_notm}}. Bucket names must be unique across all service instances and regions in {{site.data.keyword.cos_full_notm}}.  </td>
          <td>Make sure that you set `ibm.io/auto-create-bucket: false` and that you provide a bucket name that is unique in {{site.data.keyword.cos_full_notm}}. If you want to use the {{site.data.keyword.cos_full_notm}} plug-in to automatically create a bucket name for you, set `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""`. Your bucket is created with a unique name in the format `tmp-s3fs-xxxx`. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: NotFound: Not Found`</td>
          <td>You tried to access a bucket that you did not create, or the storage class and s3fs API endpoint that you specified do not match the storage class and s3fs API endpoint that were used when the bucket was created. </td>
          <td>See [Cannot access an existing bucket](#cos_access_bucket_fails). </td>
        </tr>
        <tr>
          <td>`Put https://s3-api.dal-us-geo.objectstorage.service.networklayer.com/<bucket_name>: net/http: invalid header field value "AWS4-HMAC-SHA256 Credential=1234a12a123a123a1a123aa1a123a123\n\n/20190412/us-standard/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=12aa1abc123456aabb12aas12aa123456sb123456abc" for key Authorization`</td>
          <td>The values in your Kubernetes secret are not correctly encoded to base64.  </td>
          <td>Review the values in your Kubernetes secret and encode every value to base64. You can also use the [`kubectl create secret`](/docs/containers?topic=containers-object_storage#create_cos_secret) command to create a new secret and let Kubernetes automatically encode your values to base64. </td>
        </tr>
        <tr>
          <td>`cannot access bucket <bucket_name>: Forbidden: Forbidden`</td>
          <td>You specified `ibm.io/auto-create-bucket: false` and tried to access a bucket that you did not create or the service access key or access key ID of your {{site.data.keyword.cos_full_notm}} HMAC credentials are incorrect.  </td>
          <td>You cannot access a bucket that you did not create. Create a bucket instead by setting `ibm.io/auto-create-bucket: true` and `ibm.io/bucket: ""`. If you are the owner of the bucket, see [PVC creation fails due to wrong credentials or access denied](#cred_failure) to check your credentials. </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessDenied: Access Denied`</td>
          <td>You specified `ibm.io/auto-create-bucket: true` to automatically create a bucket in {{site.data.keyword.cos_full_notm}}, but the credentials that you provided in the Kubernetes secret are assigned the **Reader** IAM service access role. This role does not allow bucket creation in {{site.data.keyword.cos_full_notm}}. </td>
          <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: AccessForbidden: Access Forbidden`</td>
          <td>You specified `ibm.io/auto-create-bucket: true` and provided a name of an existing bucket in `ibm.io/bucket`. In addition the credentials that you provided in the Kubernetes secret are assigned the **Reader** IAM service access role. This role does not allow bucket creation in {{site.data.keyword.cos_full_notm}}. </td>
          <td>To use an existing bucket, set `ibm.io/auto-create-bucket: false` and provide the name of your existing bucket in `ibm.io/bucket`. To automatically create a bucket by using your existing Kubernetes secret, set `ibm.io/bucket: ""` and follow [PVC creation fails due to wrong credentials or access denied](#cred_failure) to verify the credentials in your Kubernetes secret.</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details`</td>
          <td>The {{site.data.keyword.cos_full_notm}} secret access key of your HMAC credentials that you provided in your Kubernetes secret is not correct. </td>
          <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure).</td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`</td>
          <td>The {{site.data.keyword.cos_full_notm}} access key ID or the secret access key of your HMAC credentials that you provided in your Kubernetes secret is not correct.</td>
          <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure). </td>
        </tr>
        <tr>
          <td>`cannot create bucket <bucket_name>: CredentialsEndpointError: failed to load credentials` </br> </br> `cannot access bucket <bucket_name>: CredentialsEndpointError: failed to load credentials`</td>
          <td>The {{site.data.keyword.cos_full_notm}} API key of your IAM credentials and the GUID of your {{site.data.keyword.cos_full_notm}} service instance are not correct.</td>
          <td>See [PVC creation fails due to wrong credentials or access denied](#cred_failure). </td>
        </tr>
  </tbody>
    </table>


### Object storage: PVC or pod creation fails due to not finding the Kubernetes secret
{: #cos_secret_access_fails}

{: tsSymptoms}
When you create your PVC or deploy a pod that mounts the PVC, the creation or deployment fails.

- Example error message for a PVC creation failure:
  ```
  cannot get credentials: cannot get secret tsecret-key: secrets "secret-key" not found
  ```
  {: screen}

- Example error message for a pod creation failure:
  ```
  persistentvolumeclaim "pvc-3" not found (repeated 3 times)
  ```
  {: screen}

{: tsCauses}
The Kubernetes secret where you store your {{site.data.keyword.cos_full_notm}} service credentials, the PVC, and the pod are not all in the same Kubernetes namespace. When the secret is deployed to a different namespace than your PVC or pod, the secret cannot be accessed.

{: tsResolve}
This task requires [**Writer** or **Manager** {{site.data.keyword.cloud_notm}} IAM service role](/docs/containers?topic=containers-users#platform) for all namespaces.

1. List the secrets in your cluster and review the Kubernetes namespace where the Kubernetes secret for your {{site.data.keyword.cos_full_notm}} service instance is created. The secret must show `ibm/ibmc-s3fs` as the **Type**.
   ```
   kubectl get secrets --all-namespaces
   ```
   {: pre}

2. Check your YAML configuration file for your PVC and pod to verify that you used the same namespace. If you want to deploy a pod in a different namespace than the one where your secret exists, [create another secret](/docs/containers?topic=containers-object_storage#create_cos_secret) in that namespace.

3. Create the PVC or deploy the pod in the appropriate namespace.

<br />


### Object storage: PVC creation fails due to wrong credentials or access denied
{: #cred_failure}

{: tsSymptoms}
When you create the PVC, you see an error message similar to one of the following:

```
SignatureDoesNotMatch: The request signature we calculated does not match the signature you provided. Check your AWS Secret Access Key and signing method. For more information, see REST Authentication and SOAP Authentication for details.
```
{: screen}

```
AccessDenied: Access Denied status code: 403
```
{: screen}

```
CredentialsEndpointError: failed to load credentials
```
{: screen}

```
InvalidAccessKeyId: The AWS Access Key ID you provided does not exist in our records`
```
{: screen}

```
cannot access bucket <bucket_name>: Forbidden: Forbidden
```
{: screen}


{: tsCauses}
The {{site.data.keyword.cos_full_notm}} service credentials that you use to access the service instance might be wrong, or allow only read access to your bucket.

{: tsResolve}
1. In the navigation on the service details page, click **Service Credentials**.
2. Find your credentials, then click **View credentials**.
3. In the **iam_role_crn** section, verify that you have the `Writer` or `Manager` role. If you do not have the correct role, you must create new {{site.data.keyword.cos_full_notm}} service credentials with the correct permission.
4. If the role is correct, verify that you use the correct **access_key_id** and **secret_access_key** in your Kubernetes secret.
5. [Create a new secret with the updated **access_key_id** and **secret_access_key**](/docs/containers?topic=containers-object_storage#create_cos_secret).


<br />


### Object storage: PVC creation fails due to wrong s3fs or IAM API endpoint
{: #cos_api_endpoint_failure}

{: tsSymptoms}
When you create the PVC, the PVC remains in a pending state. After you run the `kubectl describe pvc <pvc_name>` command, you see one of the following error messages:

```
Bad value for ibm.io/object-store-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

```
Bad value for ibm.io/iam-endpoint XXXX: scheme is missing. Must be of the form http://<hostname> or https://<hostname>
```
{: screen}

{: tsCauses}
The s3fs API endpoint for the bucket that you want to use might have the wrong format, or your cluster is deployed in a location that is supported in {{site.data.keyword.containerlong_notm}} but is not yet supported by the {{site.data.keyword.cos_full_notm}} plug-in.

{: tsResolve}
1. Check the s3fs API endpoint that was automatically assigned by the `ibmc` Helm plug-in to your storage classes during the {{site.data.keyword.cos_full_notm}} plug-in installation. The endpoint is based on the location that your cluster is deployed to.  
   ```
   kubectl get sc ibmc-s3fs-standard-regional -o yaml | grep object-store-endpoint
   ```
   {: pre}

   If the command returns `ibm.io/object-store-endpoint: NA`, your cluster is deployed in a location that is supported in {{site.data.keyword.containerlong_notm}} but is not yet supported by the {{site.data.keyword.cos_full_notm}} plug-in. To add the location to the {{site.data.keyword.containerlong_notm}}, post a question in our public Slack or open an {{site.data.keyword.cloud_notm}} support case. For more information, see [Getting help and support](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help).

2. If you manually added the s3fs API endpoint with the `ibm.io/endpoint` annotation or the IAM API endpoint with the `ibm.io/iam-endpoint` annotation in your PVC, make sure that you added the endpoints in the format `https://<s3fs_api_endpoint>` and `https://<iam_api_endpoint>`. The annotation overwrites the API endpoints that are automatically set by the `ibmc` plug-in in the {{site.data.keyword.cos_full_notm}} storage classes.
   ```
   kubectl describe pvc <pvc_name>
   ```
   {: pre}

<br />


### Object storage: Cannot access an existing bucket
{: #cos_access_bucket_fails}

{: tsSymptoms}
When you create the PVC, the bucket in {{site.data.keyword.cos_full_notm}} cannot be accessed. You see an error message similar to the following:

```
Failed to provision volume with StorageClass "ibmc-s3fs-standard-regional": pvc:1b2345678b69175abc98y873e2:cannot access bucket <bucket_name>: NotFound: Not Found
```
{: screen}

{: tsCauses}
You might have used the wrong storage class to access your existing bucket, or you tried to access a bucket that you did not create. You cannot access a bucket that you did not create.

{: tsResolve}
1. From the [{{site.data.keyword.cloud_notm}} dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/), select your {{site.data.keyword.cos_full_notm}} service instance.
2. Select **Buckets**.
3. Review the **Class** and **Location** information for your existing bucket.
4. Choose the appropriate [storage class](/docs/containers?topic=containers-object_storage#cos_storageclass_reference).
5. Make sure that you set `ibm.io/auto-create-bucket: false` and that you provide the correct name of your existing bucket.

<br />


## Object storage: Accessing files with a non-root user fails
{: #cos_nonroot_access}

{: tsSymptoms}
You uploaded files to your {{site.data.keyword.cos_full_notm}} service instance by using the console or the REST API. When you try to access these files with a non-root user that you defined with `runAsUser` in your app deployment, access to the files is denied.

{: tsCauses}
In Linux, a file or a directory has three access groups: `Owner`, `Group`, and `Other`. When you upload a file to {{site.data.keyword.cos_full_notm}} by using the console or the REST API, the permissions for the `Owner`, `Group`, and `Other` are removed. The permission of each file looks as follows:

```
d--------- 1 root root 0 Jan 1 1970 <file_name>
```
{: screen}

When you upload a file by using the {{site.data.keyword.cos_full_notm}} plug-in, the permissions for the file are preserved and not changed.

{: tsResolve}
To access the file with a non-root user, the non-root user must have read and write permissions for the file. Changing the permission on a file as part of your pod deployment requires a write operation. {{site.data.keyword.cos_full_notm}} is not designed for write workloads. Updating permissions during the pod deployment might prevent your pod from getting into a `Running` state.

To resolve this issue, before you mount the PVC to your app pod, create another pod to set the correct permission for the non-root user.

1. Check the permissions of your files in your bucket.
   1. Create a configuration file for your `test-permission` pod and name the file `test-permission.yaml`.
      ```
      apiVersion: v1
      kind: Pod
      metadata:
        name: test-permission
      spec:
        containers:
        - name: test-permission
          image: nginx
          volumeMounts:
          - name: cos-vol
            mountPath: /test
        volumes:
        - name: cos-vol
          persistentVolumeClaim:
            claimName: <pvc_name>
      ```
      {: codeblock}

   2. Create the `test-permission` pod.
      ```
      kubectl apply -f test-permission.yaml
      ```
      {: pre}

   3. Log in to your pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   4. Navigate to your mount path and list the permissions for your files.
      ```
      cd test && ls -al
      ```
      {: pre}

      Example output:
      ```
      d--------- 1 root root 0 Jan 1 1970 <file_name>
      ```
      {: screen}

2. Delete the pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

3. Create a configuration file for the pod that you use to correct the permissions of your files and name it `fix-permission.yaml`.
   ```
   apiVersion: v1
   kind: Pod
   metadata:
     name: fix-permission
     namespace: <namespace>
   spec:
     containers:
     - name: fix-permission
       image: busybox
       command: ['sh', '-c']
       args: ['chown -R <nonroot_userID> <mount_path>/*; find <mount_path>/ -type d -print -exec chmod u=+rwx,g=+rx {} \;']
       volumeMounts:
       - mountPath: "<mount_path>"
         name: cos-volume
     volumes:
     - name: cos-volume
       persistentVolumeClaim:
         claimName: <pvc_name>
    ```
    {: codeblock}

3. Create the `fix-permission` pod.
   ```
   kubectl apply -f fix-permission.yaml
   ```
   {: pre}

4. Wait for the pod to go into a `Completed` state.  
   ```
   kubectl get pod fix-permission
   ```
   {: pre}

5. Delete the `fix-permission` pod.
   ```
   kubectl delete pod fix-permission
   ```
   {: pre}

5. Re-create the `test-permission` pod that you used earlier to check the permissions.
   ```
   kubectl apply -f test-permission.yaml
   ```
   {: pre}

5. Verify that the permissions for your files are updated.
   1. Log in to your pod.
      ```
      kubectl exec test-permission -it bash
      ```
      {: pre}

   2. Navigate to your mount path and list the permissions for your files.
      ```
      cd test && ls -al
      ```
      {: pre}

      Example output:
      ```
      -rwxrwx--- 1 <nonroot_userID> root 6193 Aug 21 17:06 <file_name>
      ```
      {: screen}

6. Delete the `test-permission` pod.
   ```
   kubectl delete pod test-permission
   ```
   {: pre}

7. Mount the PVC to the app with the non-root user.

   Define the non-root user as `runAsUser` without setting `fsGroup` in your deployment YAML at the same time. Setting `fsGroup` triggers the {{site.data.keyword.cos_full_notm}} plug-in to update the group permissions for all files in a bucket when the pod is deployed. Updating the permissions is a write operation and might prevent your pod from getting into a `Running` state.
   {: important}

After you set the correct file permissions in your {{site.data.keyword.cos_full_notm}} service instance, do not upload files by using the console or the REST API. Use the {{site.data.keyword.cos_full_notm}} plug-in to add files to your service instance.
{: tip}

<br />



## PVC creation fails because of missing permissions
{: #missing_permissions}

{: tsSymptoms}
When you create a PVC, the PVC remains pending. When you run `kubectl describe pvc <pvc_name>`, you see an error message similar to the following:

```
User doesn't have permissions to create or manage Storage
```
{: screen}

{: tsCauses}
The IAM API key or the IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster  does not have all the required permissions to provision persistent storage.

{: tsResolve}
1. Retrieve the IAM key or IBM Cloud infrastructure API key that is stored in the `storage-secret-store` Kubernetes secret of your cluster and verify that the correct API key is used.
   ```
   kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
   ```
   {: pre}

   Example output:
   ```
   [Bluemix]
   iam_url = "https://iam.bluemix.net"
   iam_client_id = "bx"
   iam_client_secret = "bx"
   iam_api_key = "<iam_api_key>"
   refresh_token = ""
   pay_tier = "paid"
   containers_api_route = "https://us-south.containers.bluemix.net"

   [Softlayer]
   encryption = true
   softlayer_username = ""
   softlayer_api_key = ""
   softlayer_endpoint_url = "https://api.softlayer.com/rest/v3"
   softlayer_iam_endpoint_url = "https://api.softlayer.com/mobile/v3"
   softlayer_datacenter = "dal10"
   ```
   {: screen}

   The IAM API key is listed in the `Bluemix.iam_api_key` section of your CLI output. If the `Softlayer.softlayer_api_key` is empty at the same time, then the IAM API key is used to determine your infrastructure permissions. The IAM API key is automatically set by the user who runs the first action that requires the IAM **Administrator** platform role in a resource group and region. If a different API key is set in `Softlayer.softlayer_api_key`, then this key takes precedence over the IAM API key. The `Softlayer.softlayer_api_key` is set when a cluster admin runs the `ibmcloud ks credentials-set` command.

2. If you want to change the credentials, update the API key that is used.
    1.  To update the IAM API key, use the `ibmcloud ks api-key-reset` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_api_key_reset). To update the IBM Cloud infrastructure key, use the `ibmcloud ks credential-set` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_credentials_set).
    2. Wait about 10 - 15 minutes for the `storage-secret-store` Kubernetes secret to update, then verify that the key is updated.
       ```
       kubectl get secret storage-secret-store -n kube-system -o yaml | grep slclient.toml: | awk '{print $2}' | base64 --decode
       ```
       {: pre}

3. If the API key is correct, verify that the key has the correct permission to provision persistent storage.
   1. Contact the account owner to verify the permission of the API key.
   2. As the account owner, select **Manage** > **Access (IAM)** from the navigation in the {{site.data.keyword.cloud_notm}} console.
   3. Select **Users** and find the user whose API key you want to use.
   4. From the actions menu, select **Manage user details**.
   5. Go to the **Classic infrastructure** tab.
   6. Expand the **Account** category and verify that the **Add/ Upgrade Storage (StorageLayer)** permission is assigned.
   7. Expand the **Services** category and verify that the **Storage Manage** permission is assigned.
4. Remove the PVC that failed.
   ```
   kubectl delete pvc <pvc_name>
   ```
   {: pre}

5. Re-create the PVC.
   ```
   kubectl apply -f pvc.yaml
   ```
   {: pre}


## Getting help and support
{: #storage_getting_help}

Still having issues with your cluster?
{: shortdesc}

-  In the terminal, you are notified when updates to the `ibmcloud` CLI and plug-ins are available. Be sure to keep your CLI up-to-date so that you can use all available commands and flags.
-   To see whether {{site.data.keyword.cloud_notm}} is available, [check the {{site.data.keyword.cloud_notm}} status page ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/status?selected=status).
-   Post a question in the [{{site.data.keyword.containerlong_notm}} Slack ![External link icon](../icons/launch-glyph.svg "External link icon")](https://ibm-container-service.slack.com).
    If you are not using an IBM ID for your {{site.data.keyword.cloud_notm}} account, [request an invitation](https://cloud.ibm.com/kubernetes/slack) to this Slack.
    {: tip}
-   Review the forums to see whether other users ran into the same issue. When you use the forums to ask a question, tag your question so that it is seen by the {{site.data.keyword.cloud_notm}} development teams.
    -   If you have technical questions about developing or deploying clusters or apps with {{site.data.keyword.containerlong_notm}}, post your question on [Stack Overflow ![External link icon](../icons/launch-glyph.svg "External link icon")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) and tag your question with `ibm-cloud`, `kubernetes`, and `containers`.
    -   For questions about the service and getting started instructions, use the [IBM Developer Answers ![External link icon](../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix) forum. Include the `ibm-cloud` and `containers` tags.
    See [Getting help](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) for more details about using the forums.
-   Contact IBM Support by opening a case. To learn about opening an IBM support case, or about support levels and case severities, see [Contacting support](/docs/get-support?topic=get-support-getting-customer-support).
When you report an issue, include your cluster ID. To get your cluster ID, run `ibmcloud ks clusters`. You can also use the [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) to gather and export pertinent information from your cluster to share with IBM Support.
{: tip}

