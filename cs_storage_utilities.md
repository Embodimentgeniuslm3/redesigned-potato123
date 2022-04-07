---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-18"

keywords: kubernetes, iks, local persistent storage

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



# IBM Cloud storage utilities
{: #utilities}

## Installing the IBM Cloud Block Storage Attacher plug-in (beta)
{: #block_storage_attacher}

Use the {{site.data.keyword.cloud_notm}} Block Storage Attacher plug-in to attach raw, unformatted, and unmounted block storage to a worker node in your cluster.  
{: shortdesc}

For example, you want to store your data with a software-defined storage solution (SDS), such as [Portworx](/docs/containers?topic=containers-portworx), but you do not want to use bare metal worker nodes that are optimized for SDS usage and that come with extra local disks. To add local disks to your non-SDS worker node, you must manually create your block storage devices in your {{site.data.keyword.cloud_notm}} infrastructure account and use the {{site.data.keyword.cloud_notm}} Block Volume Attacher to attach the storage to your non-SDS worker node.

The {{site.data.keyword.cloud_notm}} Block Volume Attacher plug-in creates pods on every worker node in your cluster as part of a daemon set and sets up a Kubernetes storage class that you later use to attach the block storage device to your non-SDS worker node.

Looking for instructions for how to update or remove the {{site.data.keyword.cloud_notm}} Block Volume Attacher plug-in? See [Updating the plug-in](#update_block_attacher) and [Removing the plug-in](#remove_block_attacher).
{: tip}

1.  [Follow the instructions](/docs/containers?topic=containers-helm#public_helm_install) to install the Helm client on your local machine, and install the Helm server (tiller) with a service account in your cluster.

2.  Verify that tiller is installed with a service account.

    ```
    kubectl get serviceaccount -n kube-system | grep tiller
    ```
    {: pre}

    Example output:

    ```
    NAME                                 SECRETS   AGE
    tiller                               1         2m
    ```
    {: screen}

3. Update the Helm repo to retrieve the latest version of all Helm charts in this repo.
   ```
   helm repo update
   ```
   {: pre}

4. Install the {{site.data.keyword.cloud_notm}} Block Volume Attacher plug-in. When you install the plug-in, pre-defined block storage classes are added to your cluster.
   ```
   helm install iks-charts/ibm-block-storage-attacher --name block-attacher
   ```
   {: pre}

   Example output:
   ```
   NAME:   block-volume-attacher
   LAST DEPLOYED: Thu Sep 13 22:48:18 2018
   NAMESPACE: default
   STATUS: DEPLOYED

   RESOURCES:
   ==> v1beta1/ClusterRoleBinding
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   ==> v1beta1/DaemonSet
   NAME                             DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE SELECTOR  AGE
   ibmcloud-block-storage-attacher  0        0        0      0           0          <none>         1s

   ==> v1/StorageClass
   NAME                 PROVISIONER                AGE
   ibmc-block-attacher  ibm.io/ibmc-blockattacher  1s

   ==> v1/ServiceAccount
   NAME                             SECRETS  AGE
   ibmcloud-block-storage-attacher  1        1s

   ==> v1beta1/ClusterRole
   NAME                             AGE
   ibmcloud-block-storage-attacher  1s

   NOTES:
   Thank you for installing: ibmcloud-block-storage-attacher.   Your release is named: block-volume-attacher

   Please refer Chart README.md file for attaching a block storage
   Please refer Chart RELEASE.md to see the release details/fixes
   ```
   {: screen}

5. Verify that the {{site.data.keyword.cloud_notm}} Block Volume Attacher daemon set is installed successfully.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   Example output:
   ```
   ibmcloud-block-storage-attacher-z7cv6           1/1       Running            0          19m
   ```
   {: screen}

   The installation is successful when you see one or more **ibmcloud-block-storage-attacher** pods. The number of pods equals the number of worker nodes in your cluster. All pods must be in a **Running** state.

6. Verify that the storage class for the {{site.data.keyword.cloud_notm}} Block Volume Attacher is created successfully.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   Example output:
   ```
   ibmc-block-attacher       ibm.io/ibmc-blockattacher   11m
   ```
   {: screen}

### Updating the IBM Cloud Block Storage Attacher plug-in
{: #update_block_attacher}

You can upgrade the existing {{site.data.keyword.cloud_notm}} Block Storage Attacher plug-in to the latest version.
{: shortdesc}

1. Update the Helm repo to retrieve the latest version of all helm charts in this repo.
   ```
   helm repo update
   ```
   {: pre}

2. Optional: Download the latest Helm chart to your local machine. Then, extract the package and review the `release.md` file to find the latest release information.
   ```
   helm fetch iks-charts/ibmcloud-block-storage-plugin
   ```
   {: pre}

3. Find the name of the Helm chart for the {{site.data.keyword.cloud_notm}} Block Storage Attacher plug-in.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Example output:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

4. Upgrade the {{site.data.keyword.cloud_notm}} Block Storage Attacher to latest.
   ```
   helm upgrade --force --recreate-pods <helm_chart_name> ibm-block-storage-attacher
   ```
   {: pre}

### Removing the IBM Cloud Block Volume Attacher plug-in
{: #remove_block_attacher}

If you do not want to provision and use the {{site.data.keyword.cloud_notm}} Block Storage Attacher plug-in in your cluster, you can uninstall the Helm chart.
{: shortdesc}

1. Find the name of the Helm chart for the {{site.data.keyword.cloud_notm}} Block Storage Attacher plug-in.
   ```
   helm ls | grep ibm-block-storage-attacher
   ```
   {: pre}

   Example output:
   ```
   <helm_chart_name>	1       	Wed Aug  1 14:55:15 2018	DEPLOYED	ibm-block-storage-attacher-1.0.0	default
   ```
   {: screen}

2. Delete the {{site.data.keyword.cloud_notm}} Block Storage Attacher plug-in by removing the Helm chart.
   ```
   helm delete <helm_chart_name> --purge
   ```
   {: pre}

3. Verify that the {{site.data.keyword.cloud_notm}} Block Storage Attacher plug-in pods are removed.
   ```
   kubectl get pod -n kube-system -o wide | grep attacher
   ```
   {: pre}

   The removal of the pods is successful if no pods are displayed in your CLI output.

4. Verify that the {{site.data.keyword.cloud_notm}} Block Storage Attacher storage class is removed.
   ```
   kubectl get storageclasses | grep attacher
   ```
   {: pre}

   The removal of the storage class is successful if no storage class is displayed in your CLI output.

## Automatically provisioning unformatted block storage and authorizing your worker nodes to access the storage
{: #automatic_block}

You can use the {{site.data.keyword.cloud_notm}} Block Volume Attacher plug-in to automatically add raw, unformatted, and unmounted block storage with the same configuration to all worker nodes in your cluster.
{: shortdesc}

The `mkpvyaml` container that is included in the {{site.data.keyword.cloud_notm}} Block Volume Attacher plug-in is configured to run a script that finds all worker nodes in your cluster, creates raw block storage in the {{site.data.keyword.cloud_notm}} infrastructure portal, and then authorizes the worker nodes to access the storage.

To add different block storage configurations, add block storage to a subset of worker nodes only, or to have more control over the provisioning process, choose to [manually add block storage](#manual_block).
{: tip}


1. Log in to {{site.data.keyword.cloud_notm}} and target the resource group that your cluster is in.
   ```
   ibmcloud login
   ```
   {: pre}

2.  Clone the {{site.data.keyword.cloud_notm}} Storage Utilities repo.
    ```
    git clone https://github.com/IBM/ibmcloud-storage-utilities.git
    ```
    {: pre}

3. Navigate in to the `block-storage-utilities` directory.
   ```
   cd ibmcloud-storage-utilities/block-storage-provisioner
   ```
   {: pre}

4. Open the `yamlgen.yaml` file and specify the block storage configuration that you want to add to every worker node in the cluster.
   ```
   #
   # Can only specify 'performance' OR 'endurance' and associated clause
   #
   cluster:  <cluster_name>    #  Enter the name of your cluster
   region:   <region>          #  Enter the IBM Cloud Kubernetes Service region where you created your cluster
   type:  <storage_type>       #  Enter the type of storage that you want to provision. Choose between "performance" or "endurance"
   offering: storage_as_a_service   
   # performance:
   #    - iops:  <iops>       
   endurance:
     - tier:  2                #   [0.25|2|4|10]
   size:  [ 20 ]               #   Enter the size of your storage in gigabytes
   ```
   {: codeblock}

   <table>
   <caption>Understanding the YAML file components</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
   </thead>
   <tbody>
   <tr>
   <td><code>cluster</code></td>
   <td>Enter the name of your cluster where you want to add raw block storage. </td>
   </tr>
   <tr>
   <td><code>region</code></td>
   <td>Enter the {{site.data.keyword.containerlong_notm}} region where you created your cluster. Run <code>[bxcs] cluster-get --cluster &lt;cluster_name_or_ID&gt;</code> to find the region of your cluster.  </td>
   </tr>
   <tr>
   <td><code>type</code></td>
   <td>Enter the type of storage that you want to provision. Choose between <code>performance</code> or <code>endurance</code>. For more information, see [Deciding on your block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass).  </td>
   </tr>
   <tr>
   <td><code>performance.iops</code></td>
   <td>If you want to provision `performance` storage, enter the number of IOPS. For more information, see [Deciding on your block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). If you want to provision `endurance` storage, remove this section or comment it out by adding `#` to the beginning of each line.
   </tr>
   <tr>
   <td><code>endurance.tier</code></td>
   <td>If you want to provision `endurance` storage, enter the number of IOPS per gigabyte. For example, if you want to provision block storage as it is defined in the `ibmc-block-bronze` storage class, enter 2. For more information, see [Deciding on your block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass). If you want to provision `performance` storage, remove this section or comment it out by adding `#` to the beginning of each line. </td>
   </tr>
   <tr>
   <td><code>size</code></td>
   <td>Enter the size of your storage in gigabytes. See [Deciding on your block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to find supported sizes for your storage tier. </td>
   </tr>
   </tbody>
   </table>  

5. Retrieve your IBM Cloud infrastructure user name and API key. The user name and API key are used by the `mkpvyaml` script to access the cluster.
   1. Log in to the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/).
   2. From the menu ![Menu icon](../icons/icon_hamburger.svg "Menu icon"), select **Infrastructure**.
   3. From the menu bar, select **Account** > **Users** > **User List**.
   4. Find the user whose user name and API key you want to retrieve.
   5. Click **Generate** to generate an API key or **View** to view your existing API key. A pop-up window opens that shows the infrastructure user name and API key.

6. Store the credentials in an environment variable.
   1. Add the environment variables.
      ```
      export SL_USERNAME=<infrastructure_username>
      ```
      {: pre}

      ```
      export SL_API_KEY=<infrastructure_api_key>
      ```
      {: pre}

   2. Verify that your environment variables are created successfully.
      ```
      printenv
      ```
      {: pre}

7.  Build and run the `mkpvyaml` container. When you run the container from the image, the `mkpvyaml.py` script is executed. The script adds a block storage device to every worker node in the cluster and authorizes each worker node to access the block storage device. At the end of the script, a `pv-<cluster_name>.yaml` YAML file is generated for you that you can later use to create the persistent volumes in the cluster.
    1.  Build the `mkpvyaml` container.
        ```
        docker build -t mkpvyaml .
        ```
        {: pre}
        Example output:
        ```
        Sending build context to Docker daemon   29.7kB
        Step 1/16 : FROM ubuntu:18.10
        18.10: Pulling from library/ubuntu
        5940862bcfcd: Pull complete
        a496d03c4a24: Pull complete
        5d5e0ccd5d0c: Pull complete
        ba24b170ddf1: Pull complete
        Digest: sha256:20b5d52b03712e2ba8819eb53be07612c67bb87560f121cc195af27208da10e0
        Status: Downloaded newer image for ubuntu:18.10
         ---> 0bfd76efee03
        Step 2/16 : RUN apt-get update
         ---> Running in 85cedae315ce
        Get:1 http://security.ubuntu.com/ubuntu cosmic-security InRelease [83.2 kB]
        Get:2 http://archive.ubuntu.com/ubuntu cosmic InRelease [242 kB]
        ...
        Step 16/16 : ENTRYPOINT [ "/docker-entrypoint.sh" ]
         ---> Running in 9a6842f3dbe3
        Removing intermediate container 9a6842f3dbe3
         ---> 7926f5384fc7
        Successfully built 7926f5384fc7
        Successfully tagged mkpvyaml:latest
        ```
        {: screen}
    2.  Run the container to execute the `mkpvyaml.py` script.
        ```
        docker run --rm -v `pwd`:/data -v ~/.bluemix:/config -e SL_API_KEY=$SL_API_KEY -e SL_USERNAME=$SL_USERNAME mkpvyaml
        ```
        {: pre}

        Example output:
        ```
        Unable to find image 'portworx/iks-mkpvyaml:latest' locally
        latest: Pulling from portworx/iks-mkpvyaml
        72f45ff89b78: Already exists
        9f034a33b165: Already exists
        386fee7ab4d3: Already exists
        f941b4ac6aa8: Already exists
        fe93e194fcda: Pull complete
        f29a13da1c0a: Pull complete
        41d6e46c1515: Pull complete
        e89af7a21257: Pull complete
        b8a7a212d72e: Pull complete
        5e07391a6f39: Pull complete
        51539879626c: Pull complete
        cdbc4e813dcb: Pull complete
        6cc28f4142cf: Pull complete
        45bbaad87b7c: Pull complete
        05b0c8595749: Pull complete
        Digest: sha256:43ac58a8e951994e65f89ed997c173ede1fa26fb41e5e764edfd3fc12daf0120
        Status: Downloaded newer image for portworx/iks-mkpvyaml:latest

        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087291
        kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal10-abc1d23e123456ac8b12a3c1e12b123a4
                  ORDER ID =  30087293
        kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1 10.XXX.XX.XX
        Creating Vol of size 20 with type:  endurance
                  Ordering block storage of size: 20 for host: kube-dal12-abc1d23e123456ac8b12a3c1e12b456v1
                  ORDER ID =  30085655
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  No volume yet ... for orderID :  30087291
                  Order ID =  30087291 has created VolId =  12345678
                  Granting access to volume: 12345678 for HostIP: 10.XXX.XX.XX
                  Order ID =  30087293 has created VolId =  87654321
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Vol 53002831 is not yet ready ...
                  Granting access to volume: 87654321 for HostIP: 10.XXX.XX.XX
                  Order ID =  30085655 has created VolId =  98712345
                  Granting access to volume: 98712345 for HostIP: 10.XXX.XX.XX
        Output file created as :  pv-<cluster_name>.yaml
        ```
        {: screen}

7. [Attach the block storage devices to your worker nodes](#attach_block).

## Manually adding block storage to specific worker nodes
{: #manual_block}

Use this option if you want to add different block storage configurations, add block storage to a subset of worker nodes only, or to have more control over the provisioning process.
{: shortdesc}

1. List the worker nodes in your cluster and note the private IP address and the zone of the non-SDS worker nodes where you want to add a block storage device.
   ```
   ibmcloud ks workers --cluster <cluster_name_or_ID>
   ```
   {: pre}

2. Review step 3 and 4 in [Deciding on your block storage configuration](/docs/containers?topic=containers-block_storage#block_predefined_storageclass) to choose the type, size, and number of IOPS for the block storage device that you want to add to your non-SDS worker node.    

3. Create the block storage device in the same zone that your non-SDS worker node is in.

   **Example for provisioning 20 GB endurance block storage with two IOPS per GB:**
   ```
   ibmcloud sl block volume-order --storage-type endurance --size 20 --tier 2 --os-type LINUX --datacenter dal10
   ```
   {: pre}

   **Example for provisioning 20 GB performance block storage with 100 IOPS:**
   ```
   ibmcloud sl block volume-order --storage-type performance --size 20 --iops 100 --os-type LINUX --datacenter dal10
   ```
   {: pre}

4. Verify that the block storage device is created and note the **`id`** of the volume. **Note:** If you do not see your block storage device right away, wait a few minutes. Then, run this command again.
   ```
   ibmcloud sl block volume-list
   ```
   {: pre}

   Example output:
   ```
   id         username          datacenter   storage_type                capacity_gb   bytes_used   ip_addr         lunId   active_transactions   
   123456789  IBM02SL1234567-8  dal10        performance_block_storage   20            -            161.12.34.123   0       0   
   ```
   {: screen}

5. Review the details for your volume and note the **`Target IP`** and **`LUN Id`**.
   ```
   ibmcloud sl block volume-detail <volume_ID>
   ```
   {: pre}

   Example output:
   ```
   Name                       Value   
   ID                         1234567890   
   User name                  IBM123A4567890-1   
   Type                       performance_block_storage   
   Capacity (GB)              20   
   LUN Id                     0   
   IOPs                       100   
   Datacenter                 dal10   
   Target IP                  161.12.34.123   
   # of Active Transactions   0   
   Replicant Count            0
   ```
   {: screen}

6. Authorize the non-SDS worker node to access the block storage device. Replace `<volume_ID>` with the volume ID of your block storage device that you retrieved earlier, and `<private_worker_IP>` with the private IP address of the non-SDS worker node where you want to attach the device.

   ```
   ibmcloud sl block access-authorize <volume_ID> -p <private_worker_IP>
   ```
   {: pre}

   Example output:
   ```
   The IP address 123456789 was authorized to access <volume_ID>.
   ```
   {: screen}

7. Verify that your non-SDS worker node is successfully authorized and note the **`host_iqn`**, **`username`**, and **`password`**.
   ```
   ibmcloud sl block access-list <volume_ID>
   ```
   {: pre}

   Example output:
   ```
   ID          name                 type   private_ip_address   source_subnet   host_iqn                                      username   password           allowed_host_id   
   123456789   <private_worker_IP>  IP     <private_worker_IP>  -               iqn.2018-09.com.ibm:ibm02su1543159-i106288771   IBM02SU1543159-I106288771   R6lqLBj9al6e2lbp   1146581   
   ```
   {: screen}

   The authorization is successful when the **`host_iqn`**, **`username`**, and **`password`** are assigned.

8. [Attach the block storage devices to your worker nodes](#attach_block).


## Attaching raw block storage to non-SDS worker nodes
{: #attach_block}

To attach the block storage device to a non-SDS worker node, you must create a persistent volume (PV) with the {{site.data.keyword.cloud_notm}} Block Volume Attacher storage class and the details of your block storage device.
{: shortdesc}

**Before you begin**:
- Make sure that you [automatically](#automatic_block) or [manually](#manual_block) created raw, unformatted, and unmounted block storage to your non-SDS worker nodes.
- [Log in to your account. If applicable, target the appropriate resource group. Set the context for your cluster.](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure)

**To attach raw block storage to non-SDS worker nodes**:
1. Prepare the PV creation.  
   - **If you used the `mkpvyaml` container:**
     1. Open the `pv-<cluster_name>.yaml` file.
        ```
        nano pv-<cluster_name>.yaml
        ```
        {: pre}

     2. Review the configuration for your PVs.

   - **If you manually added block storage:**
     1. Create a `pv.yaml` file. The following command creates the file with the `nano` editor.
        ```
        nano pv.yaml
        ```
        {: pre}

     2. Add the details of your block storage device to the PV.
        ```
        apiVersion: v1
        kind: PersistentVolume
        metadata:
          name: <pv_name>
          annotations:
            ibm.io/iqn: "<IQN_hostname>"
            ibm.io/username: "<username>"
            ibm.io/password: "<password>"
            ibm.io/targetip: "<targetIP>"
            ibm.io/lunid: "<lunID>"
            ibm.io/nodeip: "<private_worker_IP>"
            ibm.io/volID: "<volume_ID>"
        spec:
          capacity:
            storage: <size>
          accessModes:
            - ReadWriteOnce
          hostPath:
              path: /
          storageClassName: ibmc-block-attacher
        ```
        {: codeblock}

        <table>
        <caption>Understanding the YAML file components</caption>
        <thead>
        <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the YAML file components</th>
        </thead>
        <tbody>
      	<tr>
          <td><code>metadata.name</code></td>
      	<td>Enter a name for your PV.</td>
      	</tr>
        <tr>
        <td><code>ibm.io/iqn</code></td>
        <td>Enter the IQN host name that you retrieved earlier. </td>
        </tr>
        <tr>
        <td><code>ibm.io/username</code></td>
        <td>Enter the IBM Cloud infrastructure user name that you retrieved earlier. </td>
        </tr>
        <tr>
        <td><code>ibm.io/password</code></td>
        <td>Enter the IBM Cloud infrastructure password that you retrieved earlier. </td>
        </tr>
        <tr>
        <td><code>ibm.io/targetip</code></td>
        <td>Enter the target IP that you retrieved earlier. </td>
        </tr>
        <tr>
        <td><code>ibm.io/lunid</code></td>
        <td>Enter the LUN ID of your block storage device that you retrieved earlier. </td>
        </tr>
        <tr>
        <td><code>ibm.io/nodeip</code></td>
        <td>Enter the private IP address of the worker node where you want to attach the block storage device and that you authorized earlier to access your block storage device. </td>
        </tr>
        <tr>
          <td><code>ibm.io/volID</code></td>
        <td>Enter the ID of the block storage volume that you retrieved earlier. </td>
        </tr>
        <tr>
        <td><code>storage</code></td>
        <td>Enter the size of the block storage device that you created earlier. For example, if your block storage device is 20 gigabytes, enter <code>20Gi</code>.  </td>
        </tr>
        </tbody>
        </table>
2. Create the PV to attach the block storage device to your non-SDS worker node.
   - **If you used the `mkpvyaml` container:**
     ```
     kubectl apply -f pv-<cluster_name>.yaml
     ```
     {: pre}

   - **If you manually added block storage:**
     ```
     kubectl apply -f  block-volume-attacher/pv.yaml
     ```
     {: pre}

3. Verify that the block storage is successfully attached to your worker node.
   ```
   kubectl describe pv <pv_name>
   ```
   {: pre}

   Example output:
   ```
   Name:            kube-wdc07-cr398f790bc285496dbeb8e9137bc6409a-w1-pv1
   Labels:          <none>
   Annotations:     ibm.io/attachstatus=attached
                    ibm.io/dm=/dev/dm-1
                    ibm.io/iqn=iqn.2018-09.com.ibm:ibm02su1543159-i106288771
                    ibm.io/lunid=0
                    ibm.io/mpath=3600a09803830445455244c4a38754c66
                    ibm.io/nodeip=10.176.48.67
                    ibm.io/password=R6lqLBj9al6e2lbp
                    ibm.io/targetip=161.26.98.114
                    ibm.io/username=IBM02SU1543159-I106288771
                    kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"v1","kind":"PersistentVolume","metadata":{"annotations":{"ibm.io/iqn":"iqn.2018-09.com.ibm:ibm02su1543159-i106288771","ibm.io/lunid":"0"...
   Finalizers:      []
   StorageClass:    ibmc-block-attacher
   Status:          Available
   Claim:           
   Reclaim Policy:  Retain
   Access Modes:    RWO
   Capacity:        20Gi
   Node Affinity:   <none>
   Message:         
   Source:
       Type:          HostPath (bare host directory volume)
       Path:          /
       HostPathType:  
   Events:            <none>
   ```
   {: screen}

   The block storage device is successfully attached when the **ibm.io/dm** is set to a device ID, such as `/dev/dm/1`, and you can see **ibm.io/attachstatus=attached** in the **Annotations** section of your CLI output.

If you want to detach a volume, delete the PV. Detached volumes are still authorized to be accessed by a specific worker node and are attached again when you create a new PV with the {{site.data.keyword.cloud_notm}} Block Volume Attacher storage class to attach a different volume to the same worker node. To avoid attaching the old detached volume again, unauthorize the worker node to access the detached volume by using the `ibmcloud sl block access-revoke` command. Detaching the volume does not remove the volume from your IBM Cloud infrastructure account. To cancel the billing for your volume, you must manually [remove the storage from your IBM Cloud infrastructure account](/docs/containers?topic=containers-cleanup).
{: note}


