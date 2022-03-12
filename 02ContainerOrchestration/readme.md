# Chapter 2. Container Orchestration #

## Introduction and Learning Objectives ##

### Introduction ###

With container images, we confine the application code, its runtime, and all of its dependencies in a pre-defined format. And, with container runtimes like runC, containerd, or cri-o we can use those pre-packaged images, to create one or more containers. All of these runtimes are good at running containers on a single host. But, in practice, we would like to have a fault-tolerant and scalable solution, which can be achieved by creating a single controller/management unit, after connecting multiple nodes together. This controller/management unit is generally referred to as a container orchestrator. 

In this chapter, we will explore why we should use container orchestrators, different implementations of container orchestrators, and where to deploy them.

### Learning Objectives ###

By the end of this chapter, you should be able to:

* Define the concept of container orchestration.
* Explain the benefits of using container orchestration.
* Discuss different container orchestration options.
* Discuss different container orchestration deployment options.

## Container Orchestration ##

### What Are Containers? ###

Before we dive into container orchestration, let's review first what containers are.

Containers are an application-centric method to deliver high-performing, scalable applications on any infrastructure of your choice. Containers are best suited to deliver microservices by providing portable, isolated virtual environments for applications to run without interference from other running applications.

![ref img](https://courses.edx.org/assets/courseware/v1/1256618e247da221e7c3cc4bab9af3e3/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/container_deployment.png)

Microservices are lightweight applications written in various modern programming languages, with specific dependencies, libraries and environmental requirements. To ensure that an application has everything it needs to run successfully it is packaged together with its dependencies.

Containers encapsulate microservices and their dependencies but do not run them directly. Containers run container images.

A container image bundles the application along with its runtime, libraries, and dependencies, and it represents the source of a container deployed to offer an isolated executable environment for the application. Containers can be deployed from a specific image on many platforms, such as workstations, Virtual Machines, public cloud, etc.


### What Is Container Orchestration? ###
In Development (Dev) environments, running containers on a single host for development and testing of applications may be an option. However, when migrating to Quality Assurance (QA) and Production (Prod) environments, that is no longer a viable option because the applications and services need to meet specific requirements:

* Fault-tolerance
* On-demand scalability
* Optimal resource usage
* Auto-discovery to automatically discover and communicate with each other
* Accessibility from the outside world
* Seamless updates/rollbacks without any downtime.

Container orchestrators are tools which group systems together to form clusters where containers' deployment and management is automated at scale while meeting the requirements mentioned above.

### Container Orchestrators ### 
With enterprises containerizing their applications and moving them to the cloud, there is a growing demand for container orchestration solutions. While there are many solutions available, some are mere re-distributions of well-established container orchestration tools, enriched with features and, sometimes, with certain limitations in flexibility.

Although not exhaustive, the list below provides a few different container orchestration tools and services available today:

With enterprises containerizing their applications and moving them to the cloud, there is a growing demand for container orchestration solutions. While there are many solutions available, some are mere re-distributions of well-established container orchestration tools, enriched with features and, sometimes, with certain limitations in flexibility.

Although not exhaustive, the list below provides a few different container orchestration tools and services available today:

* Amazon Elastic Container Service : Amazon Elastic Container Service (ECS) is a hosted service provided by Amazon Web Services (AWS) to run Docker containers at scale on its infrastructure.

* Azure Container Instances : Azure Container Instance (ACI) is a basic container orchestration service provided by Microsoft Azure.

* Azure Service Fabric : Azure Service Fabric is an open source container orchestrator provided by Microsoft Azure.

* Kubernetes : Kubernetes is an open source orchestration tool, originally started by Google, today part of the Cloud Native Computing Foundation (CNCF) project.

* Marathon: Marathon is a framework to run containers at scale on Apache Mesos.

* Nomad : Nomad is the container and workload orchestrator provided by HashiCorp.

* Docker Swarm : Docker Swarm is a container orchestrator provided by Docker, Inc. It is part of Docker Engine.

Container orchestrators are also explored in another edX MOOC, Introduction to Cloud Infrastructure Technologies (LFS151x). We highly recommend that you take LFS151x.

### Why Use Container Orchestrators? ###

Although we can manually maintain a couple of containers or write scripts to manage the lifecycle of dozens of containers, orchestrators make things much easier for operators especially when it comes to managing hundreds and thousands of containers running on a global infrastructure.

Most container orchestrators can:

* Group hosts together while creating a cluster
* Schedule containers to run on hosts in the cluster based on resources availability
* Enable containers in a cluster to communicate with each other regardless of the host they are deployed to in the cluster
* Bind containers and storage resources
* Group sets of similar containers and bind them to load-balancing constructs to simplify access to containerized applications by creating a level of abstraction between the containers and the user
* Manage and optimize resource usage
* Allow for implementation of policies to secure access to applications running inside containers.

With all these configurable yet flexible features, container orchestrators are an obvious choice when it comes to managing containerized applications at scale. In this course, we will explore Kubernetes, one of the most in-demand container orchestration tools available today.

### Where to Deploy Container Orchestrators? ###

Most container orchestrators can be deployed on the infrastructure of our choice - on bare metal, Virtual Machines, on-premises, on public and hybrid cloud. Kubernetes, for example, can be deployed on a workstation, with or without a local hypervisor such as Oracle VirtualBox, inside a company's data center, in the cloud on AWS Elastic Compute Cloud (EC2) instances, Google Compute Engine (GCE) VMs, DigitalOcean Droplets, OpenStack, etc.

There are turnkey solutions which allow Kubernetes clusters to be installed, with only a few commands, on top of cloud Infrastructures-as-a-Service, such as GCE, AWS EC2, Docker Enterprise, IBM Cloud, Rancher, VMware Tanzu, and multi-cloud solutions through IBM Cloud Private or StackPointCloud.

Last but not least, there is the managed container orchestration as-a-Service, more specifically the managed Kubernetes as-a-Service solution, offered and hosted by the major cloud providers, such as Amazon Elastic Kubernetes Service (Amazon EKS), Azure Kubernetes Service (AKS), DigitalOcean Kubernetes, Google Kubernetes Engine (GKE), IBM Cloud Kubernetes Service, Oracle Container Engine for Kubernetes, or VMware Tanzu Kubernetes Grid.

### Learning Objectives (Review) ###



You should now be able to:

* Define the concept of container orchestration.
* Explain the benefits of using container orchestration.
* Discuss different container orchestration options.
* Discuss different container orchestration deployment options.

