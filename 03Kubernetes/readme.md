# Chapter 3. Kubernetes #

## Introduction and Learning Objectives ##

### Introduction ###

In this chapter, we will explain what Kubernetes is, its features, and the reasons why you should use it. We will explore the evolution of Kubernetes from Borg, Google's very own distributed workload manager. 

We will also learn about the Cloud Native Computing Foundation (CNCF), which currently hosts the Kubernetes project, along with other popular cloud-native projects, such as Prometheus, Fluentd, cri-o, containerd, Helm, Envoy, and Contour, just to name a few.

### Learning Objectives ###

By the end of this chapter, you should be able to:

* Define Kubernetes.
* Explain the reasons for using Kubernetes.
* Discuss the features of Kubernetes.
* Discuss the evolution of Kubernetes from Borg.
* Explain the role of the Cloud Native Computing Foundation.

## Kubernetes ##

### What Is Kubernetes? ###

According to the Kubernetes website,

**"Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications."**

Kubernetes comes from the Greek word κυβερνήτης, which means helmsman or ship pilot. With this analogy in mind, we can think of Kubernetes as the pilot on a ship of containers.

Kubernetes is also referred to as k8s (pronounced Kate's), as there are 8 characters between k and s.

Kubernetes is highly inspired by the Google Borg system, a container and workload orchestrator for its global operations for more than a decade. It is an open source project written in the Go language and licensed under the Apache License, Version 2.0.

Kubernetes was started by Google and, with its v1.0 release in July 2015, Google donated it to the Cloud Native Computing Foundation (CNCF). 

New Kubernetes versions are released in 3 months cycles. The current stable version is 1.19 (as of August 2020). 

### From Borg to Kubernetes ###

According to the abstract of Google's Borg paper, published in 2015,

"Google's Borg system is a cluster manager that runs hundreds of thousands of jobs, from many thousands of different applications, across a number of clusters each with up to tens of thousands of machines".

For more than a decade, Borg has been Google's secret, running its worldwide containerized workloads in production. Services we use from Google, such as Gmail, Drive, Maps, Docs, etc., they are all serviced using Borg. 

Some of the initial authors of Kubernetes were Google employees who have used Borg and developed it in the past. They poured in their valuable knowledge and experience while designing Kubernetes. Some of the features/objects of Kubernetes that can be traced back to Borg, or to lessons learned from it, are:

* API servers
* Pods
* IP-per-Pod
* Services
* Labels.

We will explore all of them, and more, in this course.

### Kubernetes Features I ###

Kubernetes offers a very rich set of features for container orchestration. Some of its fully supported features are:

* Automatic bin packing : Kubernetes automatically schedules containers based on resource needs and constraints, to maximize utilization without sacrificing availability.

* Self-healing
Kubernetes automatically replaces and reschedules containers from failed nodes. It kills and restarts containers unresponsive to health checks, based on existing rules/policy. It also prevents traffic from being routed to unresponsive containers.

* Horizontal scaling
With Kubernetes applications are scaled manually or automatically based on CPU or custom metrics utilization.

* Service discovery and Load balancing
Containers receive their own IP addresses from Kubernetes, while it assigns a single Domain Name System (DNS) name to a set of containers to aid in load-balancing requests across the containers of the set.

### Kubernetes Features II ###

Some other fully supported Kubernetes features are:

* Automated rollouts and rollbacks
Kubernetes seamlessly rolls out and rolls back application updates and configuration changes, constantly monitoring the application's health to prevent any downtime.

* Secret and configuration management
Kubernetes manages sensitive data and configuration details for an application separately from the container image, in order to avoid a re-build of the respective image. Secrets consist of sensitive/confidential information passed to the application without revealing the sensitive content to the stack configuration, like on GitHub.

* Storage orchestration
Kubernetes automatically mounts software-defined storage (SDS) solutions to containers from local storage, external cloud providers, distributed storage, or network storage systems.

* Batch execution
Kubernetes supports batch execution, long-running jobs, and replaces failed containers.

There are many additional features currently in alpha or beta phase. They will add great value to any Kubernetes deployment once they become stable features. For example, support for role-based access control (RBAC) is stable only as of the Kubernetes 1.8 release.

### Why Use Kubernetes? ###

In addition to its fully-supported features, Kubernetes is also portable and extensible. It can be deployed in many environments such as local or remote Virtual Machines, bare metal, or in public/private/hybrid/multi-cloud setups. It supports and it is supported by many 3rd party open source tools which enhance Kubernetes' capabilities and provide a feature-rich experience to its users.

Kubernetes' architecture is modular and pluggable. Not only that it orchestrates modular, decoupled microservices type applications, but also its architecture follows decoupled microservices patterns. Kubernetes' functionality can be extended by writing custom resources, operators, custom APIs, scheduling rules or plugins.

For a successful open source project, the community is as important as having great code. Kubernetes is supported by a thriving community across the world. It has more than 2,800 contributors, who, over time, have pushed over 94,000 commits. There are meet-up groups in different cities and countries which meet regularly to discuss Kubernetes and its ecosystem. There are Special Interest Groups (SIGs), which focus on special topics, such as scaling, bare metal, networking, etc. We will learn more about them in our last chapter, Kubernetes Communities.

### Kubernetes Users ###

With just a few years since its debut, many enterprises of various sizes run their workloads using Kubernetes. It is a solution for workload management in banking, education, finance and investments, gaming, information technology, media and streaming, online retail, ridesharing, telecommunications, and many other industries. There are numerous user case studies and success stories on the Kubernetes website:

* [BlaBlaCar](https://kubernetes.io/case-studies/blablacar/)
* [BlackRock](https://kubernetes.io/case-studies/blackrock/)
* [Box](https://kubernetes.io/case-studies/box/)
* [eBay](https://www.nextplatform.com/2015/11/12/inside-ebays-shift-to-kubernetes-and-containers-atop-openstack/)
* [Haufe Group](https://kubernetes.io/case-studies/haufegroup/)
* [Huawei](https://kubernetes.io/case-studies/huawei/)
* [IBM](https://kubernetes.io/case-studies/ibm/)
* [ING](https://kubernetes.io/case-studies/ing/)
* [Nokia](https://kubernetes.io/case-studies/nokia/)
* [Pearson](https://kubernetes.io/case-studies/pearson/)
* [Wikimedia](https://kubernetes.io/case-studies/wikimedia/)
* And many more.

### Cloud Native Computing Foundation (CNCF) ###

The Cloud Native Computing Foundation (CNCF) is one of the projects hosted by the Linux Foundation. CNCF aims to accelerate the adoption of containers, microservices, and cloud-native applications.

CNCF hosts a multitude of projects, with more to be added in the future. CNCF provides resources to each of the projects, but, at the same time, each project continues to operate independently under its pre-existing governance structure and with its existing maintainers. Projects within CNCF are categorized based on achieved status: Sandbox, Incubating, and Graduated. At the time of this writing, a dozen projects had reached Graduated status with many more Incubating and in the Sandbox.

Graduated projects:

* Kubernetes for container orchestration
* Prometheus for monitoring
* Envoy for service mesh
* CoreDNS for service discovery
* containerd for container runtime
* Fluentd for logging
* Harbor for registry
* Helm for package management
* Vitess for cloud-native storage
* Jaeger for distributed tracing
* TUF for software updates
* TiKV for key/value store

Incubating projects:

* CRI-O for container runtime
* Linkerd for service mesh
* Contour for ingress
* etcd for key/value store
* gRPC for remote procedure call (RPC)
* CNI for networking API
* Rook for cloud-native storage
* Notary for security
* NATS for messaging
* OpenTracing for distributed tracing
* Open Policy Agent for policy
* And many more.

There are many projects in the CNCF Sandbox geared towards metrics, monitoring, identity, scripting, serverless, nodeless, edge, expecting to achieve Incubating and possibly Graduated status. While many active projects are preparing for takeoff, others are being archived once they become less active. The first archived project is the rkt container runtime. 

The projects under CNCF cover the entire lifecycle of a cloud-native application, from its execution using container runtimes, to its monitoring and logging. This is very important to meet the goals of CNCF.

### CNCF and Kubernetes ###

For Kubernetes, the Cloud Native Computing Foundation:

* Provides a neutral home for the Kubernetes trademark and enforces proper usage
* Provides license scanning of core and vendor code
* Offers legal guidance on patent and copyright issues
* Creates open source learning curriculum, training, and certification for both Kubernetes administrators (CKA) and application developers (CKAD)
* Manages a software conformance working group
* Actively markets Kubernetes
* Supports ad hoc activities
* Sponsors conferences and meetup events.

### Learning Objectives (Review) ###

You should now be able to:

* Define Kubernetes.
* Explain the reasons for using Kubernetes.
* Discuss the features of Kubernetes.
* Discuss the evolution of Kubernetes from Borg.
* Explain the role of the Cloud Native Computing Foundation.

 