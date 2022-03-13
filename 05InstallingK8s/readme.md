### Chapter 5. Installing Kubernetes ###

## Introduction and Learning Objectives ##

### Introduction ###

In this chapter we will explore Kubernetes cluster deployment considerations. First, we will learn about Kubernetes cluster configuration options, followed by infrastructure requirements and installation tools specific to various cluster deployment models.

### Learning Objectives ###

By the end of this chapter, you should be able to:

* Discuss Kubernetes configuration options.
* Discuss infrastructure considerations before installing Kubernetes.
* Discuss infrastructure choices for a Kubernetes cluster deployment.
* Review Kubernetes installation tools and resources.

## Installing Kubernetes ##

### Kubernetes Configuration ###

Kubernetes can be installed using different cluster configurations. The major installation types are described below:

* All-in-One Single-Node Installation
In this setup, all the master and worker components are installed and running on a single-node. While it is useful for learning, development, and testing, it should not be used in production. Minikube is an installation tool originally aimed at single-node cluster installations, and we are going to explore it in future chapters.

* Single-Master and Multi-Worker Installation
In this setup, we have a single-master node running a stacked etcd instance. Multiple worker nodes can be managed by the master node.

* Single-Master with Single-Node etcd, and Multi-Worker Installation
In this setup, we have a single-master node with an external etcd instance. Multiple worker nodes can be managed by the master node.

* Multi-Master and Multi-Worker Installation
In this setup, we have multiple master nodes configured for High-Availability (HA), with each master node running a stacked etcd instance. The etcd instances are also configured in an HA etcd cluster and, multiple worker nodes can be managed by the HA masters.

* Multi-Master with Multi-Node etcd, and Multi-Worker Installation
In this setup, we have multiple master nodes configured in HA mode, with each master node paired with an external etcd instance. The external etcd instances are also configured in an HA etcd cluster, and multiple worker nodes can be managed by the HA masters. This is the most advanced cluster configuration recommended for production environments. 

As the Kubernetes cluster's complexity grows, so does its hardware and resources requirements. While we can deploy Kubernetes on a single host for learning, development, and possibly testing purposes, the community recommends multi-host environments that support High-Availability control plane setups and multiple worker nodes for client workload. 

### Infrastructure for Kubernetes Installation ###

Once we decide on the installation type, we need to decide on the infrastructure. Infrastructure related decisions are typically guided by the desired environment type, either learning or production environment. For infrastructure, we need to decide on the following:

* Should we set up Kubernetes on bare metal, public cloud, private, or hybrid cloud?
* Which underlying OS should we use? Should we choose a Linux distribution such as RHEL, CentOS, CoreOS, or Windows?
* Which networking solution should we use?

Explore the [Kubernetes documentation](https://kubernetes.io/docs/setup/) for details on choosing the right solution. 

### Localhost Installation ###

There are a variety of installation tools allowing us to deploy single- or multi-node Kubernetes clusters on our workstations. While not an exhaustive list, below we enumerate a few popular ones:

* [Minikube](https://minikube.sigs.k8s.io/docs/) - single-node local Kubernetes cluster, recommended for a learning environment deployed on a single host. 
* [Kind](https://kind.sigs.k8s.io/docs/) - multi-node Kubernetes cluster deployed in Docker containers acting as Kubernetes nodes, recommended for a learning environment. 
* [Docker Desktop]((https://www.docker.com/products/docker-desktop)) - including a local Kubernetes cluster for Docker users. 
* [MicroK8s](https://microk8s.io/) - local and cloud Kubernetes cluster, from Canonical. 
* [K3S](https://k3s.io/) - lightweight Kubernetes cluster for local, cloud, edge, IoT deployments, from Rancher. 

Minikube is the easiest and most preferred method to create an all-in-one Kubernetes setup locally. We will be using it extensively in this course to manage a single-node cluster. A newer experimental Minikube feature is the multi-node cluster support, which will not be explored at this time. 

### On-Premise Installation ###

Kubernetes can be installed on-premise on VMs and bare metal.

* On-Premise VMs
Kubernetes can be installed on VMs created via Vagrant, VMware vSphere, KVM, or another Configuration Management (CM) tool in conjunction with a hypervisor software. There are different tools available to automate the installation, such as [Ansible](https://kubernetes.io/blog/2019/03/15/kubernetes-setup-using-ansible-and-vagrant/) or [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/). 

* On-Premise Bare Metal
Kubernetes can be installed on on-premise bare metal, on top of different operating systems, like RHEL, CoreOS, CentOS, Fedora, Ubuntu, etc. Most of the tools used to install Kubernetes on VMs can be used with bare metal installations as well. 

### Cloud Installation ###

Kubernetes can be installed and managed on almost any cloud environment for production:

* Hosted Solutions
With Hosted Solutions, any given software is completely managed by the provider, while the user pays hosting and management charges. Popular vendors providing hosted solutions for Kubernetes are (listed in alphabetical order):
    * [Alibaba Cloud Container Service for Kubernetes](https://www.alibabacloud.com/product/kubernetes) (ACK) 
    * [Amazon Elastic Kubernetes Service](https://aws.amazon.com/eks/) (EKS) 
    * [Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/) (AKS) 
    * [DigitalOcean Kubernetes](https://www.digitalocean.com/products/kubernetes/) 
    * [Google Kubernetes Engine](https://cloud.google.com/kubernetes-engine/) (GKE) 
    * [IBM Cloud Kubernetes Service ](https://www.ibm.com/cloud/container-service/)
    * [Oracle Cloud Container Engine for Kubernetes](https://www.oracle.com/cloud/compute/container-engine-kubernetes.html) (OKE) 
    * [Platform9 Managed Kubernetes](https://platform9.com/managed-kubernetes/) (PMK) 
    * [Red Hat OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift) 
    * [VMware Tanzu Kubernetes](https://tanzu.vmware.com/kubernetes-grid). 

* Turnkey Cloud Solutions
Below are only a few of the [Turnkey Cloud Solutions](https://kubernetes.io/docs/setup/production-environment/turnkey-solutions/) (listed in alphabetical order), to install Kubernetes on an underlying IaaS platform, such as:
    * Alibaba Cloud 
    * Amazon AWS (AWS EC2) 
    * Google Compute Engine (GCE)
    * IBM Cloud Private.

* Turnkey On-Premise Solutions
The On-Premise Solutions install Kubernetes on secure internal private clouds:
    * [GKE On-Prem](https://cloud.google.com/anthos/gke/docs/on-prem) part of Google Cloud [Anthos](https://cloud.google.com/anthos)
    * [IBM Private Cloud](https://www.ibm.com/cloud/learn/introduction-to-private-cloud)
    * [OpenShift Container Platform](https://www.openshift.com/products/container-platform) by Red Hat. 

### Kubernetes Installation Tools/Resources ###

While discussing installation configuration and the underlying infrastructure, let's take a look at some useful installation tools available:

* kubeadm
[kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/) is a first-class citizen on the Kubernetes ecosystem. It is a secure and recommended method to bootstrap a multi-node production ready Highly Available Kubernetes cluster, on-prem or in the cloud. Kubeadm can also bootstrap a single-node cluster for learning. It has a set of building blocks to setup the cluster, but it is easily extendable to add more features. Please note that kubeadm does not support the provisioning of hosts. 

* kubespray
With [kubespray](https://kubernetes.io/docs/setup/production-environment/tools/kubespray/) (formerly known as kargo), we can install Highly Available production ready Kubernetes clusters on AWS, GCE, Azure, OpenStack, vSphere, or bare metal. Kubespray is based on Ansible, and is available on most Linux distributions. It is a [Kubernetes Incubator](https://github.com/kubernetes-sigs/kubespray) project. 

* kops
With kops, we can create, upgrade, and maintain production-grade, Highly Available Kubernetes clusters from the command line. It can provision the machines as well. Currently, AWS is officially supported. Support for GCE and OpenStack* is in beta, VMware vSphere is in alpha support, and other platforms are planned for the future. Explore the [kops project](https://github.com/kubernetes/kops) for more details. 

* kube-aws
With [kube-aws](https://github.com/kubernetes-incubator/kube-aws) we can create, upgrade and destroy Kubernetes clusters on AWS from the command line. Kube-aws is also a Kubernetes Incubator project. 

In addition, for a manual installation approach, the [Kubernetes The Hard Way](https://github.com/kelseyhightower/kubernetes-the-hard-way) GitHub project by [Kelsey Hightower](https://twitter.com/kelseyhightower) is an extremely helpful installation guide and resource. The project aims to teach all the detailed steps involved in the bootstrapping of a Kubernetes cluster. 

### Kubernetes on Windows ###

The Windows operating system plays a key role in running and managing enterprise applications and services. With that in mind, the Kubernetes community worked very hard to bring Windows support to Kubernetes. 

With the release of Kubernetes v1.14, Windows was successfully introduced as a [supported](https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/) production ready operating system only for worker nodes of a Kubernetes cluster. This enabled Kubernetes to support the deployment of Windows containers in the cluster, either as a dedicated Windows cluster, or a shared cluster with Windows nodes running alongside Linux nodes. Keep in mind, however, that the control plane nodes are limited to running on Linux only, with no plans to extend the support to Windows master nodes.

With Windows Server 2019 being the only Windows OS supported by Kubernetes, the same container workload orchestration tool can [schedule](https://kubernetes.io/docs/setup/production-environment/windows/user-guide-windows-containers/) and deploy both Linux and Windows containers in the same cluster. The user is responsible to configure the workload scheduling according to the expected OS, that is to schedule Linux and Windows containers on nodes with their respective operating systems when nodes of each OS are found in the same Kubernetes cluster.

## Learning Objectives (Review) ##

### Learning Objectives (Review) ###

You should now be able to:

* Discuss Kubernetes configuration options.
* Discuss infrastructure considerations before installing Kubernetes.
* Discuss infrastructure choices for a Kubernetes cluster deployment.
* Review Kubernetes installation tools and resources.

