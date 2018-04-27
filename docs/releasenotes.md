---
id: releasenotes
title: Changelog
sidebar_label: Changelog
---
<center><p style="padding: 20px; margin: 20px 0; border-radius: 3px; background-color: #eeeeee;"><strong>
  Our docs for version 0.5 release are getting updated. The documentation you are currently viewing is a static snapshot of earlier version for the same 0.5 release.  Click here for [latest](https://docs.openebs.io) version.
</strong></p></center>

## OpenEBS Release Version 0.5.3

### Issues Fixed in v0.5.3

- Fixed usage of StoragePool issue when rbac settings are applied [1189](https://github.com/openebs/openebs/issues/1189).
- Fixed hardcoded maya-apiserver-service name to a configurable value as it resulted in conflict with other services running on the same cluster [1227](https://github.com/openebs/openebs/issues/1227).
- Fixed issue where the volume status was displaying only one replica even though there were three replicas configured in mayactl version 0.5.1. [1275](https://github.com/openebs/openebs/issues/1275).
- Fixed an issue where the OpenEBS iSCSI volume showed progressive increase in the memory consumed by the controller pod [1298](https://github.com/openebs/openebs/issues/1298).

### Known Issues in v0.5.3

For a list of known issues, go to [v0.5.3 known issues](https://github.com/openebs/openebs/issues?q=is%3Aopen+is%3Aissue+label%3Arelease-note%2Fopen). 

### Enhancement to Documentation

The OpenEBS documentation is now available at https://docs.openebs.io/. You can provide your feedback comments by clicking the **Feedback** button provided on every page.

## OpenEBS Release Version 0.5.2

This release fixes an issue to allow maya-apiserver and openebs-provisioner to work with Kubernetes non-SSL configuration.

### Issue Fixed in v0.5.2

You can now set the non-SSL Kubernetes endpoints to use by specifying the ENV variables OPENEBS_IO_KUBE_CONFIG and OPENEBS_IO_K8S_MASTER on maya-apiserver and openebs-provisioner [1184](https://github.com/openebs/openebs/issues/1184).

To use the above ENV variables, the following image versions must be used:

- *openebs/m-apiserver:0.5.2*: OpenEBS Maya API Server along with the latest maya cli.
- *openebs/openebs-k8s-provisioner:0.5.2*: Dynamic OpenEBS Volume Provisioner for Kubernetes.

OpenEBS Release Version 0.5.1
-----------------------------

### Issues Fixed in v0.5.1

* Fixed the inter-operability issues of connecting to OpenEBS Volume from CentOS iscsi Initiator [1087](https://github.com/openebs/openebs/issues/1087).


* Fixed openebs-k8s-provisioner that must be launched in non-default namespace [1055](https://github.com/openebs/openebs/issues/#1055).
* Update the documentation with steps to use OpenEBS on OpenShift Kubernetes Cluster [1102](https://github.com/openebs/openebs/issues/1102) and Kubernetes on CentOS [1104](https://github.com/openebs/openebs/issues/1104).
* Update helm charts to use OpenEBS 0.5.1 [1100](https://github.com/openebs/openebs/issues/#1100)

### Known Limitations 

-   Requires Kubernetes 1.7.5+
-   Requires iscsi initiator that must be installed in the Kubernetes nodes or kubelet container
-   Not recommended for mission critical workloads
-   Not recommended for performance sensitive workloads. Efforts are ongoing to improve performance.

### Enhancements to Installation

Execute the following command to install OpenEBS using kubectl.

````
kubectl apply -f <https://raw.githubusercontent.com/openebs/openebs/v0.5.1/k8s/openebs-operator.yaml
````

Execute the following command to install OpenEBS using helm. 

```
helm repo add openebs-charts https://openebs.github.io/charts/ 
helm repo update 
helm install openebs-charts/openebs
```

### Images

The following images are installed when you install OpenEBS.

-   *openebs/jiva:0.5.1* : Containerized Storage Controller
-   *openebs/m-apiserver:0.5.1* : OpenEBS Maya API Server along with the latest maya cli.
-   *openebs/openebs-k8s-provisioner:0.5.1* : Dynamic OpenEBS Volume Provisioner for Kubernetes.
-   *openebs/m-exporter:0.5.1* : OpenEBS Volume metrics exporter.

### Setup OpenEBS Volume Monitoring

If you are running your own Prometheus, please update it with the following job configuration: 

```
- job_name: 'openebs-volumes' 
  scheme: http 
  kubernetes_sd_configs: 
  - role: pod 
  relabel_configs: 
  - source_labels: [__meta_kubernetes_pod_label_monitoring] 
    regex:volume_exporter_prometheus 
    action: keep 
  - source_labels: [__meta_kubernetes_pod_name] 
    action: replace 
    target_label: kubernetes_pod_name 
  - source_labels: [__meta_kubernetes_pod_label_vsm] 
    action: replace 
    target_label: openebs_pv 
  - source_labels: [__meta_kubernetes_pod_container_port_number] 
    action: drop 
    regex: '(.*)9501' 
  - source_labels: [__meta_kubernetes_pod_container_port_number] 
    action: drop 
    regex: '(.*)3260

```

If you do not have Prometheus running, you can use the following yaml file to run Prometheus and Grafana. 

```
kubectl apply -f https://raw.githubusercontent.com/openebs/openebs/v0.5.0/k8s/openebs-monitoring-pg.yaml
```

You can import the following grafana-dashboard file to view the OpenEBS Volume metrics.

<https://raw.githubusercontent.com/openebs/openebs/v0.5.0/k8s/openebs-pg-dashboard.json>

OpenEBS Release Version 0.5.0
-----------------------------

### New v0.5.0 Features

-   Storage Policy Enforcement Framework that allows DevOps teams to deploy a customized storage. Some of the storage policies supported are for -

    -   using a custom Storage Engine like Jiva
    -   exposing volume metrics in Prometheus format using a side-car to volume controller
    -   defining capacity

    -   defining the persistent storage location like */var/openebs* (default) or a directory mounted on EBS or GPD and so on
-   Extend OpenEBS API Server to expose volume snapshot api
-   Support for deploying OpenEBS using helm charts
-   Sample Prometheus configuration for collecting OpenEBS Volume Metrics
-   Sample Grafana OpenEBS Volume Dashboard using the prometheus Metrics
-   Sample Deployment YAMLs and corresponding storage classes for different types of applications. For a more detailed list, see Deployment Examples of YAMLs using OpenEBS\_
-   Sample Deployment YAMLs for launching the Kubernetes Dashboard for a preview of the changes made by the OpenEBS Team to the Kubernetes Dashboard. See, Kubernetes Dashboard/UI Updates\_ for the PRs raised and merged.
-   Sample Deployment YAMLs for Prometheus and Grafana if they are not already a part of your deployment
-   Several documentation and code re-factoring changes for improving code quality

### Enhancements for v0.5.0

-   Support for defining storage policies as parameters on a StorageClass (openebs/maya \#136 \#180 openebs/external-storage \#6)
-   OpenEBS Volume Exporter (m-exporter) container is introduced which can be launched as a sidecar to the OpenEBS Volume Controller. (openebs/maya \#186 \#200)
-   Support for Storage Pool policy, to allow users to specify the location (directory) to store the data (openebs/maya \#192)
-   Include additional metrics along with IOPS, Throughput, Latency, Block Size:

    -   VolName, Uptime (openebs/jiva \#30)

    - Total Capacity, Used Capacity (openebs/jiva \#29)
-   Added /metrics endpoint to OpenEBS Volume - controller and replica volumes, to gather volume control api statistics (openebs/jiva \#17)
-   Extend OpenEBS apiserver api to manage snapshots (openebs/maya \#164)
-   openebs-provisioner to connect with m-apiserver through non-default namespace (openebs/external-storage \#12)
-   Optimize the container images for size (openebs/jiva \#24, openebs/maya \#119)
-   Improve the logging mechanism for openebs-provisioner (openebs/external-storage \#10)
-   OpenEBS Volume Controller and Replicas can be scheduled on specific nodes using taints (openebs/maya \#106)

### Deployment Examples of YAMLs using OpenEBS

Following are some of the deployment examples that are supported in this release.

-   MongoDB (openebs/openebs \#488)
-   Cassandra (openebs/openebs \#504)
-   MySQL Replication Cluster (openebs/openebs \#543)
-   Redis (openebs/openebs \#561)
-   Kafka (openebs/openebs \#610)
-   RabbitMQ (openebs/openebs \#662)
-   EFK (openebs/openebs \#682)
-   CockroachDB (openebs/openebs \#682)
-   Jenkins (openebs/openebs \#741)
-   MySQL Syncrhonous Replication Cluster using Galera (openebs/openebs \#803)
-   Crunchy Postgress (openebs/openebs \#895)
-   Couchbase (openebs/openebs \#901)

### Issues Fixed in v0.5.0

-   OpenEBS volume fails to attach when the capacity is provided as Gi (openebs/jiva \#32)
-   Jiva iSCSI target returns a negative "target portal group tag" on discovery (openebs/jiva \#28)
-   Add resiliency to openebs-provisioner to handle intermittent issues with apiserver (openebs/external-storage \#7)

### Known Issues in v0.5.0

For a list of known issues, go to [v0.5.0 known issues](https://github.com/openebs/openebs/labels/documentation/release-note-open).

### Kubernetes Dashboard/UI Updates

-   Show PersistentVolumes created for a given StorageClass (kubernetes/dashboard \#2495)
-   Show PersistentVolumeClaims used by a Pod (kubernetes/dashboard \#2515)
-   Make the PVC, PV, StorageClass clickable from PVC and PV pages (kubernetes/dashboard \#2520 \#2560)

### CI Updates

-   Include feature test to verify

    -   snapshot create-revert workflow (openebs/openebs \#859)
    -   basic high-availability workflow (openebs/openebs \#868)

    -   snapshot workflow for percona-mysql application (openebs/openebs \#885)
-   Run volume provisioning tests using minikube on openebs/maya code commits (openebs/maya \#142)
-   MySQL Test Containers with automated replication cluster setup (openebs/test-storage \#21 \#22 openebs/openebs \#543)
-   Sysbench Test Container (openebs/test-storage \#25)
-   Mechanism to avoid running deployment CI when commits are made to documentation (openebs/openebs \#889)
-   Improve the slack notifications based on Jenkins CI status (openebs/openebs \#603)
-   Improvise on the CI run time (openebs/openebs \#626 \#779)
-   Make the contain tag (on which CI should be run) configurable for Ansible playbook (openebs/openebs \#739 \#905)
-   Few other incremental enhancements/bugfixes (openebs/openebs \#481 \#598 \#522 \#623 \#627 \#629 \#912)

### Notes for Contributors

-   openebs/mayaserver has been deprecated and code migrated to openebs/maya (openebs/maya \#95)
-   openebs/maya repository now hosts all the OpenEBS native control plane components like m-apiserver, mayactl, m-exporter. The build and directory structures are accordingly modified. Please see
    openebs/maya/cmd/\<component-name\> for a starting point.
-   openebs/maya now uses GO as a build tool and dep for dependency management (openebs/maya \#96 )
-   CLI is moving toward COBRA. All the new components use COBRA, mayactl except for the volume stats command, while others now support COBRA CLI. In the next release, we look forward to move the
    remaining commands to COBRA CLI and converge on a single CLI framework. (openebs/maya \#388)
-   The OpenEBS control plane components are designed to be plugged into different COs. The components/framework is being refactored to make it easy to support additional COs in the future release. Some of these framework changes are warranting to remove the code dependent on the deprecated usage (primarily used by nomad) is being removed.
-   openebs/maya/cmd/maya-agent was introduced to manage and automate the local storage functionality. This component is being renamed to openebs/maya/cmd/maya-nodebot and is primarily intended to augment the functionality already provided by k8s local storage manager and
    node-exporter (openebs/openebs \#194)

### Limitation

Running in Azure K8s Clusters (not verified)

OpenEBS Release Version 0.4.0
-----------------------------

You can try out OpenEBS v0.4.0 on your Kubernetes Cluster using the [quick start guide](/docs/next/overview.html).

### Downloads for v0.4.0

The following OpenEBS v0.4.0 containers are available at the [Docker Hub](https://hub.docker.com/r/openebs/).

-   openebs/jiva:0.4.0 : Storage Controller
-   openebs/m-apiserver:0.4.0 : OpenEBS Maya API Server along with the latest maya cli.
-   openebs/openebs-k8s-provisioner:0.4.0 : Dynamic OpenEBS Volume Provisioner for Kubernetes.

### New v0.4.0 Features

-   Maya CLI Support for managing snapshots for OpenEBS Volumes
-   Maya CLI Support for obtaining the capacity usage statistics from OpenEBS Volumes
-   OpenEBS Volume - Dynamic Provisioner is merged into kubernetes-incubator/external-storage project
-   OpenEBS Maya API Server uses the Kubernetes scheduler logic to place OpenEBS Volume Replicas on different nodes
-   OpenEBS Maya API Server can be customized by providing ENV options through K8s YAML file for default replica count and jiva image to be used
-   OpenEBS user documentation is available at <https://docs.openebs.io>
-   OpenEBS now supports deployment on AWS, along with previously supported Google Cloud and On-premise setups
-   OpenEBS Vagrant boxes are upgraded to support Kubernetes version 1.7.5
-   OpenEBS can now be deployed within a minikube setup

### Issues Fixed in v0.4.0

-   \#166 (<https://github.com/edorid>): openebs-k8s-provisioner goes into crashloopbackoff, during the first volume creation
-   \#176 (<https://github.com/maikotz>): OpenEBS PV is unreachable after one of the replica becomes unreachable.

### Known Issues in v0.4.0

-   \#633 (<https://github.com/openebs/openebs/issues/633>):

**Issue:**

Setting up OpenEBS with Kubernetes using Minikube on the Ubuntu host displayed the following error. *error: error validating "openebs-operator.yaml": error validating data: unknown object type schema.GroupVersionKind{Group:"", Version:"v1", Kind:"ServiceAccount"}; if you choose to ignore these errors, turn validation off with --validate=false*

**Resolution**

1.  Download a specific/compatible version, by replacing the **$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt>)** portion of the command with a specific version.

    For example, to download v1.8.0 on Linux, enter the following command. 

    ```
    curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl>
    ```

2.  Make the kubectl binary executable. 

    ```
    chmod +x ./kubectl
    ```

3.  Move the binary in to your PATH.  

    ```
    sudo mv ./kubectl /usr/local/bin/kubectl
    ```

    ​

### CI Updates with v0.4.0

-   Support for on-premise Jenkins CI for performing e2e tests
-   iSCSI compliance tests are run as part of the CI
-   CI can now be extended using a framework developer for running storage benchmark tests with vdbench or fio.
-   CI has been extended to run Percona Benchmarking tests on Kubernetes.

### Deprecated with v0.4.0

The maya cli options (setup-omm, setup-osh, omm-status, osh-status) to setup and manage dedicated OpenEBS setup is removed. Starting with v0.4.0, only hyperconvergence with Kubernetes is supported.

### Notes for Contributors

-   OpenEBS user documentation is currently being moved into *openebs/openebs/documentation*
-   OpenEBS developer documentation is currently being added to *openebs/openebs/contribute*
-   The deployment and e2e functionality will continue to be located in *openebs/k8s* and *openebs/e2e* respectively.
-   openebs/maya will act as a single repository for hosting different OpenEBS Storage Control plane (orchestration) components.
-   New /metrics handlers are being added to OpenEBS components to allow integration into tools like Prometheus.
-   *openebs/maya/cmd/maya-agent* which will be deployed as a deamon-set running along-side kubelet is being developed. maya-agent will augument the kubelet with storage management functionality.


<!-- Hotjar Tracking Code for https://docs.openebs.io -->
<script>
   (function(h,o,t,j,a,r){
       h.hj=h.hj||function(){(h.hj.q=h.hj.q||[]).push(arguments)};
       h._hjSettings={hjid:785693,hjsv:6};
       a=o.getElementsByTagName('head')[0];
       r=o.createElement('script');r.async=1;
       r.src=t+h._hjSettings.hjid+j+h._hjSettings.hjsv;
       a.appendChild(r);
   })(window,document,'https://static.hotjar.com/c/hotjar-','.js?sv=');
</script>