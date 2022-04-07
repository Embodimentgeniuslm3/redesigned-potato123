---

copyright:
  years: 2014, 2019
lastupdated: "2019-07-22"

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


# Locations
{: #regions-and-zones}

You can deploy community Kubernetes or OpenShift clusters worldwide by using {{site.data.keyword.containerlong}} . When you create a cluster, its resources remain in the location that you deploy the cluster to. To work with your cluster, you can access the {{site.data.keyword.containershort_notm}} via a global API endpoint.
{:shortdesc}

![{{site.data.keyword.containerlong_notm}} locations](images/locations.png)

_{{site.data.keyword.containerlong_notm}} locations_

{{site.data.keyword.cloud_notm}} resources used to be organized into regions that were accessed via [region-specific endpoints](#bluemix_regions). Use the [global endpoint](#endpoint) instead.
{: deprecated}

## {{site.data.keyword.containerlong_notm}} locations
{: #locations}

{{site.data.keyword.cloud_notm}} resources are organized into a hierarchy of geographic locations. {{site.data.keyword.containerlong_notm}} is available in a subset of these locations, including all six worldwide multizone-capable regions. Free clusters are available in only select locations. Other {{site.data.keyword.cloud_notm}} services might be available globally or within a specific location.
{: shortdesc}

Using the [Red Hat OpenShift on IBM Cloud beta](/docs/openshift?topic=openshift-getting-started)? You can create OpenShift clusters in two multizone metro areas: Washington, DC and London. Supported zones are wdc04, wdc06, wdc07, lon04, lon05, and lon06.
{: preview}

### Available locations
{: #available-locations}

To list available {{site.data.keyword.containerlong_notm}} locations, use the `ibmcloud ks supported-locations` command.
{: shortdesc}

The following image is used as an example to explain how {{site.data.keyword.containerlong_notm}} locations are organized.

![Organization of {{site.data.keyword.containerlong_notm}} locations](images/cs_regions_hierarchy.png)

<table summary="The table shows organization of {{site.data.keyword.containerlong_notm}} locations. Rows are to be read from the left to right, with the location type in column one, an example of the type in column two, and the description in column three.">
<caption>Organization of {{site.data.keyword.containerlong_notm}} locations.</caption>
  <thead>
  <th>Type</th>
  <th>Example</th>
  <th>Description</th>
  </thead>
  <tbody>
    <tr>
      <td>Geography</td>
      <td>North America (`na`)</td>
      <td>An organizational grouping that is based on geographic continents.</td>
    </tr>
    <tr>
      <td>Country</td>
      <td>Canada (`ca`)</td>
      <td>The location's country within the geography.</td>
    </tr>
    <tr>
      <td>Metro</td>
      <td>Mexico City (`mex-cty`), Dallas (`dal`)</td>
      <td>The name of a city where 1 or more data centers (zones) are located. A metro can be multizone-capable and have multizone-capable data centers, such as Dallas, or can have only single zone data centers, such as Mexico City. If you create a cluster in a multizone-capable metro, the Kubernetes master and worker nodes can be spread across zones for high availability.</td>
    </tr>
    <tr>
      <td>Data center (zone)</td>
      <td>Dallas 12 (`dal12`)</td>
      <td>A physical location of the compute, network, and storage infrastructure and related cooling and power that host cloud services and applications. Clusters can be spread across data centers, or zones, in an multizone architecture for high availability. Zones are isolated from each other, which ensures no shared single point of failure.</td>
    </tr>
  </tbody>
  </table>

### Single zone and multizone locations in {{site.data.keyword.containerlong_notm}}
{: #zones}

The following tables list the available single and multizone locations in {{site.data.keyword.containerlong_notm}}. Note that in certain metros, you can provision a cluster as a single zone or multizone cluster. Also, free clusters are only available in select geographies as only single zone clusters with one worker node.
{: shortdesc}

* **Multizone**: If you create a cluster in a multizone metro location, the replicas of your highly available Kubernetes master are automatically spread across zones. You have the option to spread your worker nodes across zones to protect your apps from a zone failure.
* **Single zone**: If you create a cluster in a single zone, or data center, location, you can create multiple worker nodes but you cannot spread them across zones. The highly available master includes three replicas on separate hosts, but is not spread across zones.

To quickly determine whether a zone is multizone-capable, your can run `ibmcloud ks supported-locations` and look for the value in the `Multizone Metro` column.
{: tip}

{{site.data.keyword.cloud_notm}} resources used to be organized into regions that were accessed via [region-specific endpoints](#bluemix_regions). The tables list the previous regions for informational purposes. Going forward, you can use the [global endpoint](#endpoint) to move toward a region-less architecture.
{: deprecated}

**Multizone metro locations**

<table summary="The table shows the available multizone metro locations in {{site.data.keyword.containerlong_notm}}. Rows are to be read from the left to right.  Column one is the geography that the location is in, column two is the country of the location, column three is the metro of the location, column four is the data center, and column five is the deprecated region that the location used to be organized into.">
<caption>Available multizone metro locations in {{site.data.keyword.containerlong_notm}}.</caption>
  <thead>
  <th>Geography</th>
  <th>Country</th>
  <th>Metro</th>
  <th>Data center</th>
  <th>Deprecated region</th>
  </thead>
  <tbody>
    <tr>
      <td>Asia Pacific</td>
      <td>Australia</td>
      <td>Sydney</td>
      <td>syd01, syd04, syd05</td>
      <td>AP South (`ap-south`, `au-syd`)</td>
    </tr>
    <tr>
      <td>Asia Pacific</td>
      <td>Japan</td>
      <td>Tokyo</td>
      <td>tok02, tok04, tok05</td>
      <td>AP North (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Germany</td>
      <td>Frankfurt</td>
      <td>fra02, fra04, fra05</td>
      <td>EU Central (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>United Kingdom</td>
      <td>London</td>
      <td>lon04, lon05`*`, lon06</td>
      <td>UK South (`uk-south`, `eu-gb`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>United States</td>
      <td>Dallas</td>
      <td>dal10, dal12, dal13</td>
      <td>US South (`us-south`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>United States</td>
      <td>Washington, D.C.</td>
      <td>wdc04, wdc06, wdc07</td>
      <td>US East (`us-east`)</td>
    </tr>
  </tbody>
  </table>

**Single zone data center locations**

<table summary="The table shows the available single zone data center locations in {{site.data.keyword.containerlong_notm}}. Rows are to be read from the left to right. Column one is the geography that the location is in, column two is the country of the location, column three is the metro of the location, column four is the data center, and column five is the deprecated region that the location used to be organized into.">
<caption>Available single zone locations in {{site.data.keyword.containerlong_notm}}.</caption>
  <thead>
  <th>Geography</th>
  <th>Country</th>
  <th>Metro</th>
  <th>Data center</th>
  <th>Deprecated region</th>
  </thead>
  <tbody>
    <tr>
      <td>Asia Pacific</td>
      <td>Australia</td>
      <td>Melbourne</td>
      <td>mel01</td>
      <td>AP South (`ap-south`, `au-syd`)</td>
    </tr>
    <tr>
      <td>Asia Pacific</td>
      <td>Australia</td>
      <td>Sydney</td>
      <td>syd01, syd04, syd05</td>
      <td>AP South (`ap-south`, `au-syd`)</td>
    </tr>
    <tr>
      <td>Asia Pacific</td>
      <td>China</td>
      <td>Hong Kong<br>SAR of the PRC</td>
      <td>hkg02</td>
      <td>AP North (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asia Pacific</td>
      <td>India</td>
      <td>Chennai</td>
      <td>che01</td>
      <td>AP North (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asia Pacific</td>
      <td>Japan</td>
      <td>Tokyo</td>
      <td>tok02, tok04, tok05</td>
      <td>AP North (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asia Pacific</td>
      <td>Korea</td>
      <td>Seoul</td>
      <td>seo01</td>
      <td>AP North (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Asia Pacific</td>
      <td>Singapore</td>
      <td>Singapore</td>
      <td>sng01</td>
      <td>AP North (`ap-north`, `jp-tok`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>France</td>
      <td>Paris</td>
      <td>par01</td>
      <td>EU Central (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Germany</td>
      <td>Frankfurt</td>
      <td>fra02, fra04, fra05</td>
      <td>EU Central (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Italy</td>
      <td>Milan</td>
      <td>mil01</td>
      <td>EU Central (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>The Netherlands</td>
      <td>Amsterdam</td>
      <td>ams03</td>
      <td>EU Central (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>Norway</td>
      <td>Oslo</td>
      <td>osl</td>
      <td>EU Central (`eu-central`, `eu-de`)</td>
    </tr>
    <tr>
      <td>Europe</td>
      <td>United Kingdom</td>
      <td>London</td>
      <td>lon02`*`, lon04, lon05`*`, lon06</td>
      <td>UK South (`uk-south`, `eu-gb`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>Canada</td>
      <td>Montreal</td>
      <td>mon01</td>
      <td>US East (`us-east`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>Canada</td>
      <td>Toronto</td>
      <td>tor01</td>
      <td>US East (`us-east`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>Mexico</td>
      <td>Mexico City</td>
      <td>mex01</td>
      <td>US South (`us-south`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>United States</td>
      <td>Dallas</td>
      <td>dal10, dal12, dal13</td>
      <td>US South (`us-south`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>United States</td>
      <td>San Jose</td>
      <td>sjc03, sjc04</td>
      <td>US South (`us-south`)</td>
    </tr>
    <tr>
      <td>North America</td>
      <td>United States</td>
      <td>Washington, D.C.</td>
      <td>wdc04, wdc06, wdc07</td>
      <td>US East (`us-east`)</td>
    </tr>
    <tr>
      <td>South America</td>
      <td>Brazil</td>
      <td>São Paulo</td>
      <td>sao01</td>
      <td>US South (`us-south`)</td>
    </tr>
  </tbody>
  </table>

`*` lon05 replaces lon02. New clusters must use lon05, which supports highly available masters that are spread across zones.
{: note}

### Single zone clusters
{: #regions_single_zone}

In a single zone cluster, your cluster's resources remain in the zone in which the cluster is deployed. The following image highlights the relationship of single zone cluster components with an example of the Toronto, Canada `tor01` location.
{: shortdesc}

<img src="/images/location-cluster-resources.png" width="650" alt="Understanding where your cluster resources reside" style="width:650px; border-style: none"/>

_Understanding where your single zone cluster resources are._

1.  Your cluster's resources, including the master and worker nodes, are in the same data center that you deployed the cluster to. When you initiate local container orchestration actions, such as `kubectl` commands, the information is exchanged between your master and worker nodes within the same zone.

2.  If you set up other cluster resources, such as storage, networking, compute, or apps running in pods, the resources and their data remain in the zone that you deployed your cluster to.

3.  When you initiate cluster management actions, such as using `ibmcloud ks` commands, basic information about the cluster (such as name, ID, user, the command) is routed through a regional endpoint via the global endpoint. Regional endpoints are located in the nearest multizone metro region. In this example, the metro region is Washington, D.C.

### Multizone clusters
{: #regions_multizone}

In a multizone cluster, your cluster's resources are spread across multiple zones for higher availability.
{: shortdesc}

1.  Worker nodes are spread across multiple zones in the metro location to provide more availability for your cluster. The Kubernetes master replicas are also spread across zones. When you initiate local container orchestration actions, such as `kubectl` commands, the information is exchanged between your master and worker nodes through the global endpoint.

2.  Other cluster resources, such as storage, networking, compute, or apps running in pods, vary in how they deploy to the zones in your multizone cluster. For more information, review these topics:
    *   Setting up [file storage](/docs/containers?topic=containers-file_storage#add_file) and [block storage](/docs/containers?topic=containers-block_storage#add_block) in multizone clusters, or [choosing a multizone persistent storage solution](/docs/containers?topic=containers-storage_planning#persistent_storage_overview).
    *   [Enabling public or private access to an app by using a network load balancer (NLB) service in a multizone cluster](/docs/containers?topic=containers-loadbalancer#multi_zone_config).
    *   [Managing network traffic by using Ingress](/docs/containers?topic=containers-ingress-about).
    *   [Increasing the availability of your app](/docs/containers?topic=containers-app#increase_availability).

3.  When you initiate cluster management actions, such as using [`ibmcloud ks` commands](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli), basic information about the cluster (such as name, ID, user, the command) is routed through the global endpoint.

### Free clusters
{: #regions_free}

Free clusters are limited to specific locations.
{: shortdesc}

**Creating a free cluster in the CLI**: Before you create a free cluster, you must target a region by running `ibmcloud ks region-set`. Your cluster is created in a metro within the region that you target: the Sydney metro in `ap-south`, the Frankfurt metro in `eu-central`, the London metro in `uk-south`, or the Dallas metro in `us-south`. Note that you cannot specify a zone within the metro.

**Creating a free cluster in the {{site.data.keyword.cloud_notm}} console**: When you use the console, you can select a geography and a metro location in the geography. You can select the Dallas metro in North America, the Frankfurt or London metros in Europe, or the Sydney metro in Asia Pacific. Your cluster is created in a zone within the metro you choose.

To work with a free cluster in the London metro, you must target the EU Central regional API by running `ibmcloud ks init --host https://eu-gb.containers.cloud.ibm.com`.
{: important}

<br />


## Accessing the global endpoint
{: #endpoint}

You can organize your resources across {{site.data.keyword.cloud_notm}} services by using {{site.data.keyword.cloud_notm}} locations (formerly called regions). For example, you can create a Kubernetes cluster by using a private Docker image that is stored in your {{site.data.keyword.registryshort_notm}} of the same location. To access these resources, you can use the global endpoints and filter by location.
{:shortdesc}

### Logging in to {{site.data.keyword.cloud_notm}}
{: #login-ic}

When you log in to the {{site.data.keyword.cloud_notm}} (`ibmcloud`) command line, you are prompted to select a region. However, this region does not affect the {{site.data.keyword.containerlong_notm}} plug-in (`ibmcloud ks`) endpoint, which still uses the global endpoint. Note that you do still need to target the resource group that your cluster is in if it is not in the default resource group.
{: shortdesc}

To log in to the {{site.data.keyword.cloud_notm}} global API endpoint and target the resource group that your cluster is in:
```
ibmcloud login -a https://cloud.ibm.com -g <nondefault_resource_group_name>
```
{: pre}

### Logging in to {{site.data.keyword.containerlong_notm}}
{: #login-iks}

When you log in to {{site.data.keyword.cloud_notm}}, you can access the {{site.data.keyword.containershort_notm}}. To help you get started, check out the following resources for using the {{site.data.keyword.containerlong_notm}} CLI and API.
{: shortdesc}

**{{site.data.keyword.containerlong_notm}} CLI**:
* [Set up your CLI to use the `ibmcloud ks` plug-in](/docs/containers?topic=containers-cs_cli_install#cs_cli_install).
* [Configure your CLI to connect to a particular cluster and run `kubectl` commands](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).

By default, you are logged in to the global {{site.data.keyword.containerlong_notm}} endpoint, `https://containers.cloud.ibm.com`.

When you use the new global functionality in the {{site.data.keyword.containerlong_notm}} CLI, consider the following changes from the legacy region-based functionality.

* Listing resources:
  * When you list resources, such as with the `ibmcloud ks clusters`, `ibmcloud ks subnets`, or `ibmcloud ks zones` commands, resources in all locations are returned. To filter resources by a specific location, certain commands include a `--locations` flag. For example, if you filter clusters for the `dal` metro, multizone clusters in that metro and single-zone clusters in data centers (zones) within that metro are returned. If you filter clusters for the `dal10` data center (zone), multizone clusters that have a worker node in that zone and single-zone clusters in that zone are returned. Note that you can pass one location or a comma-separated list of locations.
    Example to filter by location:
    ```
    ibmcloud ks clusters --locations dal
    ```
    {: pre}
  * Other commands do not return resources in all locations. To run `credential-set/unset/get`, `api-key-reset`, and `vlan-spanning-get` commands, you must specify a region in the `--region`.

* Working with resources:
  * When you use the global endpoint, you can work with resources that you have access permissions to in any location, even if you set a region by running `ibmcloud ks region-set` and the resource that you want to work with is in another region.
  * If you have clusters with the same name in different regions, you can either use the cluster ID when you run commands or set a region with the `ibmcloud ks region-set` command and use the cluster name when you run commands.

* Legacy functionality:
  * If you need to list and work with resources from one region only, you can use the `ibmcloud ks init` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_init) to target a regional endpoint instead of the global endpoint.
    Example to target the US South regional endpoint:
    ```
    ibmcloud ks init --host https://us-south.containers.cloud.ibm.com
    ```
    {: pre}
  * To use the global functionality, you can use the `ibmcloud ks init` command again to target the global endpoint. Example to target the global endpoint again:
    ```
    ibmcloud ks init --host https://containers.cloud.ibm.com
    ```
    {: pre}

</br></br>
**{{site.data.keyword.containerlong_notm}} API**:
* [Get started with the API](/docs/containers?topic=containers-cs_cli_install#cs_api).
* [View documentation on the API commands](https://containers.cloud.ibm.com/global/swagger-global-api/).
* Generate a client of the API to use in automation by using the [`swagger.json` API](https://containers.cloud.ibm.com/global/swagger-global-api/swagger.json).

To interact with the global {{site.data.keyword.containerlong_notm}} API, enter the command type and append `global/v1/command` to the endpoint.

Example of `GET /clusters` global API:
```
GET https://containers.cloud.ibm.com/global/v1/clusters
```
{: codeblock}

</br>

If you need to specify a region in an API call, remove the `/global` parameter from the path and pass the region name in the `X-Region` header. To list available regions, run `ibmcloud ks regions`.

<br />



## Deprecated: Previous {{site.data.keyword.cloud_notm}} region and zone structure
{: #bluemix_regions}

Previously, your {{site.data.keyword.cloud_notm}} resources were organized into regions. Regions are a conceptual tool to organize zones, and can include zones (data centers) in different countries and geographies. The following table maps the previous {{site.data.keyword.cloud_notm}} regions, {{site.data.keyword.containerlong_notm}} regions, and {{site.data.keyword.containerlong_notm}} zones. Multizone-capable zones are in bold.
{: shortdesc}

Region-specific endpoints are deprecated. Use the [global endpoint](#endpoint) instead. If you must use regional endpoints, [set the `IKS_BETA_VERSION` environment variable in the {{site.data.keyword.containerlong_notm}} plug-in to `0.2`](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_beta).
{: deprecated}

| {{site.data.keyword.containerlong_notm}} region | Corresponding {{site.data.keyword.cloud_notm}} regions | Available zones in the region |
| --- | --- | --- |
| AP North (standard clusters only) | Tokyo | che01, hkg02, seo01, sng01, **tok02, tok04, tok05** |
| AP South | Sydney | mel01, **syd01, syd04, syd05** |
| EU Central | Frankfurt | ams03, **fra02, fra04, fra05**, mil01, osl01, par01 |
| UK South | London | lon02, **lon04, lon05, lon06** |
| US East (standard clusters only) | Washington DC | mon01, tor01, **wdc04, wdc06, wdc07** |
| US South | Dallas | **dal10, dal12, dal13**, mex01, sjc03, sjc04, sao01 |
{: caption="Corresponding {{site.data.keyword.containershort_notm}} and {{site.data.keyword.cloud_notm}} regions, with zones. Multizone-capable zones are in bold." caption-side="top"}

By using {{site.data.keyword.containerlong_notm}} regions, you can create or access Kubernetes clusters in a region other than the {{site.data.keyword.cloud_notm}} region that you are logged in to. {{site.data.keyword.containerlong_notm}} region endpoints refer specifically to the {{site.data.keyword.containerlong_notm}}, not {{site.data.keyword.cloud_notm}} as a whole.

You might want to log in to another {{site.data.keyword.containerlong_notm}} region for the following reasons:
  * You created {{site.data.keyword.cloud_notm}} services or private Docker images in one region and want to use them with {{site.data.keyword.containerlong_notm}} in another region.
  * You want to access a cluster in a region that is different from the default {{site.data.keyword.cloud_notm}} region that you are logged in to.

To quickly switch regions, use the `ibmcloud ks region-set` [command](/docs/containers?topic=containers-cli-plugin-kubernetes-service-cli#cs_region-set).
