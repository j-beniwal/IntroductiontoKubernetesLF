# Chapter 8. Kubernetes Building Blocks #

## Introduction and Learning Objectives ##

### Introduction ###

In this chapter, we will explore the Kubernetes object model and describe some of its fundamental building blocks, such as Pods, ReplicaSets, Deployments, Namespaces, etc. We will also discuss the essential role of Labels and Selectors in a microservices driven architecture as they logically group decoupled objects together.

### Learning Objectives ###

By the end of this chapter, you should be able to:

* Describe the Kubernetes object model.
* Discuss Kubernetes building blocks, e.g. Pods, ReplicaSets, Deployments,
* Namespaces.
* Discuss Labels and Selectors.

## Kubernetes Building Blocks ##

### Kubernetes Object Model ###

Kubernetes has a very rich object model, representing different persistent entities in the Kubernetes cluster. Those entities describe:

* What containerized applications we are running
* The nodes where the containerized applications are deployed
* Application resource consumption
* Policies attached to applications, like restart/upgrade policies, fault tolerance, etc.

With each object, we declare our intent, or the desired state of the object, in the spec section. The Kubernetes system manages the status section for objects, where it records the actual state of the object. At any given point in time, the Kubernetes Control Plane tries to match the object's actual state to the object's desired state.

Examples of Kubernetes objects are Pods, ReplicaSets, Deployments, Namespaces, etc. We will explore them next.

Examples of Kubernetes objects are Pods, ReplicaSets, Deployments, Namespaces, etc. We will explore them next.

When creating an object, the object's configuration data section from below the spec field has to be submitted to the Kubernetes API server. The API request to create an object must have the spec section, describing the desired state, as well as other details. Although the API server accepts object definition files in a JSON format, most often we provide such files in a YAML format which is converted by kubectl in a JSON payload and sent to the API server.

Below is an example of a [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) object's configuration manifest in YAML format:


    apiVersion: apps/v1
    kind: Deployment
    metadata:
        name: nginx-deployment
        labels:
            app: nginx
    spec:
        replicas: 3
        selector:
            matchLabels:
                app: nginx
        template:
            metadata:
                labels:
                    app: nginx
            spec:
                containers:
                - name: nginx
                image: nginx:1.15.11
                ports:
                - containerPort: 80


 
* apiVersion: This field is the first required field, and it specifies the API endpoint on the API server which we want to connect to; it must match an existing version for the object type defined. 
* kind: The second required field is specifying the object type - in our case it is Deployment, but it can be Pod, Replicaset, Namespace, Service, etc.  
* metadata: The third required field holds the object's basic information, such as name, labels, namespace, etc. 
* Our example shows two spec fields (spec and spec.template.spec). The fourth required field spec marks the beginning of the block defining the desired state of the Deployment object. In our example, we are requesting that 3 replicas, or 3 instances of the Pod, are running at any given time. 
* The Pods are created using the Pod Template defined in spec.template. A nested object, such as the Pod being part of a Deployment, retains its metadata and spec and loses the apiVersion and kind - both being replaced by template. 
* In spec.template.spec, we define the desired state of the Pod. Our Pod creates a single container running the nginx:1.15.11 image from Docker Hub.

Once the Deployment object is created, the Kubernetes system attaches the status field to the object and populates it with all necessary status fields. 

### Pods ###

A Pod is the smallest and simplest Kubernetes object. It is the unit of deployment in Kubernetes, which represents a single instance of the application. A Pod is a logical collection of one or more containers, which:

* Are scheduled together on the same host with the Pod
* Share the same network namespace, meaning that they share a single IP address originally assigned to the Pod
* Have access to mount the same external storage (volumes).

![pod ref img](https://courses.edx.org/assets/courseware/v1/ccc5ba54a8a06ac2a87fe447bb53dcf1/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/pods-1-2-3-4.svg)

Pods are ephemeral in nature, and they do not have the capability to self-heal themselves. That is the reason they are used with controllers which handle Pods' replication, fault tolerance, self-healing, etc. Examples of controllers are Deployments, ReplicaSets, ReplicationControllers, etc. We attach a nested Pod's specification to a controller object using the Pod Template, as we have seen in the previous section.

Below is an example of a Pod object's configuration manifest in YAML format:

    apiVersion: v1
    kind: Pod
    metadata:
        name: nginx-pod
        labels:
            app: nginx
    spec:
        containers:
        - name: nginx
          image: nginx:1.15.11
          ports:
          - containerPort: 80

* The apiVersion field must specify v1 for the Pod object definition. 
* The second required field is kind specifying the Pod object type. 
* The third required field metadata, holds the object's name and label. 
* The fourth required field spec marks the beginning of the block defining the desired state of the Pod object - also named the PodSpec. 
* Our Pod creates a single container running the nginx:1.15.11 image from Docker Hub. The containerPort field specifies the container port to be exposed by Kubernetes resources for inter-application access or external client access.

### Labels ###

[Labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) are key-value pairs attached to Kubernetes objects (e.g. Pods, ReplicaSets, Nodes, Namespaces, Persistent Volumes). Labels are used to organize and select a subset of objects, based on the requirements in place. Many objects can have the same Label(s). Labels do not provide uniqueness to objects. Controllers use Labels to logically group together decoupled objects, rather than using objects' names or IDs.

![labels ref img](https://courses.edx.org/assets/courseware/v1/6669997d43534cbd2f251a57ebe0587c/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/Labels.png)

In the image above, we have used two Label keys: app and env. Based on our requirements, we have given different values to our four Pods. The Label env=dev logically selects and groups the top two Pods, while the Label app=frontend logically selects and groups the left two Pods. We can select one of the four Pods - bottom left, by selecting two Labels: app=frontend AND env=qa.

### Label Selectors ###

Controllers use [Label Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors) to select a subset of objects. Kubernetes supports two types of Selectors:


* Equality-Based Selectors
  Equality-Based Selectors allow filtering of objects based on Label keys and values. Matching is achieved using the =, == (equals, used interchangeably), or != (not equals) operators. For example, with env==dev or env=dev we are selecting the objects where the env Label key is set to value dev. 
* Set-Based Selectors
  Set-Based Selectors allow filtering of objects based on a set of values. We can use in, notin operators for Label values, and exist/does not exist operators for Label keys. For example, with env in (dev,qa) we are selecting objects where the env Label is set to either dev or qa; with !app we select objects with no Label key app.

![label selector](https://courses.edx.org/assets/courseware/v1/2f83a85bea7ffd9fd861195725d7f146/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/Selectors.png)

### ReplicationControllers ###

Although no longer a recommended controller, a [ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/) ensures a specified number of replicas of a Pod is running at any given time, by constantly comparing the actual state with the desired state of the managed application. If there are more Pods than the desired count, the replication controller randomly terminates the number of Pods exceeding the desired count, and, if there are fewer Pods than the desired count, then the replication controller requests additional Pods to be created until the actual count matches the desired count. Generally, we do not deploy a Pod independently, as it would not be able to re-start itself if terminated in error because a Pod misses the much desired self-healing feature that Kubernetes otherwise promises. The recommended method is to use some type of a controller to run and manage Pods.

The default recommended controller is the [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) which configures a [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) controller to manage Pods' lifecycle.

### ReplicaSets I ###

A [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) is, in part, the next-generation ReplicationController, as it implements the replication and self-healing aspects of the ReplicationController. ReplicaSets support both equality- and set-based Selectors, whereas ReplicationControllers only support equality-based Selectors. 

With the help of a ReplicaSet, we can scale the number of Pods running a specific application container image. Scaling can be accomplished manually or through the use of an [autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/).

Below we graphically represent a ReplicaSet, with the replica count set to 3 for a specific Pod template. Pod-1, Pod-2, and Pod-3 are identical, running the same application container image, being cloned from the same Pod template. For now, the current state matches the desired state. 

![replica ref imgage](https://courses.edx.org/assets/courseware/v1/bdb9c27c39d027fc22133456a2882fa6/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/Replica_Set1.png)

### ReplicaSets II ###

Let's continue with the same ReplicaSet example and assume that one of the Pods is forced to unexpectedly terminate (due to insufficient resources, timeout, etc.), causing the current state to no longer match the desired state. 

![replica ref img](https://courses.edx.org/assets/courseware/v1/010297d9a0a76859de8110273eb56ae1/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/ReplicaSet2.png)

### ReplicaSets III ###

The ReplicaSet detects that the current state is no longer matching the desired state and triggers a request for an additional Pod to be created, thus ensuring that the current state matches the desired state. 

![replica set ref img](https://courses.edx.org/assets/courseware/v1/454b618e38084900bf247aecec0b3679/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/ReplicaSet3.png)

ReplicaSets can be used independently as Pod controllers but they only offer a limited set of features. A set of complementary features are provided by Deployments, the recommended controllers for the orchestration of Pods. Deployments manage the creation, deletion, and updates of Pods. A Deployment automatically creates a ReplicaSet, which then creates a Pod. There is no need to manage ReplicaSets and Pods separately, the Deployment will manage them on our behalf.

We will take a closer look at Deployments next.

### Deployments I ###

Deployment objects provide declarative updates to Pods and ReplicaSets. The DeploymentController is part of the master node's controller manager, and as a controller it also ensures that the current state always matches the desired state. It allows for seamless application updates and rollbacks through rollouts and rollbacks, and it directly manages its ReplicaSets for application scaling. 

In the following example, a new Deployment creates ReplicaSet A which then creates 3 Pods, with each Pod Template configured to run one nginx:1.7.9 container image. In this case, the ReplicaSet A is associated with nginx:1.7.9 representing a state of the Deployment. This particular state is recorded as Revision 1.

![deployment ref img](https://courses.edx.org/assets/courseware/v1/5b2c8cbd6bed63c68f6a7d2566615d7f/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/Deployment_Updated.png)

### Deployments II ###

In time, we need to push updates to the application managed by the Deployment object. Let's change the Pods' Template and update the container image from nginx:1.7.9 to nginx:1.9.1. The Deployment triggers a new ReplicaSet B for the new container image versioned 1.9.1 and this association represents a new recorded state of the Deployment, Revision 2. The seamless transition between the two ReplicaSets, from ReplicaSet A with 3 Pods versioned 1.7.9 to the new ReplicaSet B with 3 new Pods versioned 1.9.1, or from Revision 1 to Revision 2, is a Deployment rolling update. 

A rolling update is triggered when we update specific properties of the Pod Template for a deployment. While updating the container image, container port, volumes, and mounts would trigger a new Revision, other operations like scaling or labeling the deployment do not trigger a rolling update, thus do not change the Revision number.

Once the rolling update has completed, the Deployment will show both ReplicaSets A and B, where A is scaled to 0 (zero) Pods, and B is scaled to 3 Pods. This is how the Deployment records its prior state configuration settings, as Revisions. 

![deployment ref img](https://courses.edx.org/assets/courseware/v1/979a990d505485a3ad502836ca5f1078/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/ReplikaSet_B.png)

### Deployments III ###

Once ReplicaSet B and its 3 Pods versioned 1.9.1 are ready, the Deployment starts actively managing them. However, the Deployment keeps its prior configuration states saved as Revisions which play a key factor in the rollback capability of the Deployment - returning to a prior known configuration state. In our example, if the performance of the new nginx:1.9.1 is not satisfactory, the Deployment can be rolled back to a prior Revision, in this case from Revision 2 back to Revision 1 running nginx:1.7.9 once again.

![ref img](https://courses.edx.org/assets/courseware/v1/d309705d109411db1c71c20e9a7de118/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/Deployment_points_to_ReplikaSet.png)

### Namespaces ###

If multiple users and teams use the same Kubernetes cluster we can partition the cluster into virtual sub-clusters using Namespaces. The names of the resources/objects created inside a Namespace are unique, but not across Namespaces in the cluster.

To list all the Namespaces, we can run the following command:

    $ kubectl get namespaces

Generally, Kubernetes creates four Namespaces out of the box: kube-system, kube-public, kube-node-lease, and default. The kube-system Namespace contains the objects created by the Kubernetes system, mostly the control plane agents. The default Namespace contains the objects and resources created by administrators and developers, and objects are assigned to it by default unless another Namespace name is provided by the user. kube-public is a special Namespace, which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about the cluster. The newest Namespace is kube-node-lease which holds node lease objects used for node heartbeat data. Good practice, however, is to create additional Namespaces, as desired, to virtualize the cluster and isolate users, developer teams, applications, or tiers. 

Namespaces are one of the most desired features of Kubernetes, securing its lead against competitors, as it provides a solution to the multi-tenancy requirement of today's enterprise development teams. 

[Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/) help users limit the overall resources consumed within Namespaces, while [LimitRanges](https://kubernetes.io/docs/concepts/policy/limit-range/) help limit the resources consumed by Pods or Containers in a Namespace. We will briefly cover quota management in a later chapter.

## Learning Objectives (Review) ##

### Learning Objectives (Review) ###

You should now be able to:

* Describe the Kubernetes object model.
* Discuss Kubernetes building blocks, e.g. Pods, ReplicaSets, Deployments,
* Namespaces.
* Discuss Labels and Selectors.


