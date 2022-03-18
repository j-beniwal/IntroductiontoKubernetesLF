# Chapter 6. Minikube - A Local Kubernetes Cluster #

## Introduction and Learning Objectives ##

### Introduction ###

As mentioned earlier, Minikube is the easiest and most popular method to run an all-in-one Kubernetes cluster in a virtual machine (VM) locally on our workstations. Minikube is the tool responsible for the installation of Kubernetes components, cluster bootstrapping, and cluster tear-down when no longer needed. It includes a few additional tools aimed to ease the user interaction with the Kubernetes cluster, but nonetheless, it initializes for us an all-in-one single-node Kubernetes cluster. While the latest Minikube release also supports multi-node Kubernetes clusters, this feature is still experimental. 

In this chapter, we will explore the requirements to install Minikube locally on our workstation, together with the installation instructions to set it up on local Linux, macOS, and Windows operating systems. 

### Learning Objectives ###

By the end of this chapter, you should be able to:

* Understand Minikube.
* Install Minikube on local Linux, macOS, and Windows workstation.
* Verify the local installation.

## Minikube - A Local Kubernetes Cluster ##

### Requirements for Running Minikube ###

Minikube is installed and runs directly on a local Linux, macOS, or Windows workstation. However, in order to fully take advantage of all the features Minikube has to offer, a [Type-2 Hypervisor](https://en.wikipedia.org/wiki/Hypervisor) or a Container Runtime should be installed on the local workstation, to run in conjunction with Minikube. This does not mean that we need to create any VMs with guest operating systems with this Hypervisor or Containers. Minikube builds all its infrastructure as long as the Type-2 Hypervisor, or a Container Runtime, is installed on our workstation. 

Minikube uses [libmachine](https://github.com/docker/machine/tree/master/libmachine) to invoke the Hypervisor that provisions the VM which hosts a single-node Kubernetes cluster, or the Container Runtime to run the Container that hosts the cluster. Minikube then uses [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/) to provision the Kubernetes cluster. Although VirtualBox is Minikube's original hypervisor driver, several other drivers are supported by Minikube when provisioning the environment in which the Kubernetes cluster is to be bootstrapped. Thus we need to make sure that we have the necessary hardware and software required by Minikube to build our environment. 

Below we outline the requirements to run Minikube on our local workstation: 

* VT-x/AMD-v virtualization must be enabled on the local workstation in BIOS, and/or verify if virtualization is supported by your workstation's OS.

* kubectl
kubectl is a binary used to access and manage any Kubernetes cluster. It is installed through Minikube and accessed through the minikube command, or it can be installed separately. We will explore kubectl in more detail in future chapters.

* Type-2 Hypervisor or Container Runtime
    * On Linux [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [KVM2](https://www.linux-kvm.org/page/Main_Page) or Docker and Podman runtimes
    * On macOS [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [HyperKit](https://github.com/moby/hyperkit), [VMware Fusion](http://www.vmware.com/products/fusion.html), [Parallels](https://minikube.sigs.k8s.io/docs/drivers/parallels/) or Docker runtime
    * On Windows VirtualBox, Hyper-V, or Docker runtime.
    
    NOTE: Minikube supports a --driver=none (on Linux) option that runs the Kubernetes components directly on the host OS and not inside a VM. With this option a Docker installation is required and a Linux OS on the local workstation, but no hypervisor installation. If you use --driver=none in Debian or its derivatives, be sure to download .deb Docker packages instead of the snap package which does not work with Minikube. In addition to hypervisors, Minikube also supports a limited number of container runtimes, with the --driver=docker (on Linux, macOS, and Windows) and --driver=podman (on Linux) options, to install and run the Kubernetes cluster on top of a container runtime. 

* 
            Internet connection at least on first Minikube run - to download packages, dependencies, updates and pull images needed to initialize the Minikube Kubernetes cluster. Subsequent runs will require an internet connection only when new Docker images need to be pulled from a container registry or when deployed containerized applications need it. Once an image has been pulled it can be reused without an internet connection.

In this chapter, we use VirtualBox as hypervisor on all three operating systems - Linux, macOS, and Windows, to allow Minikube to provision the VM which hosts the single-node Kubernetes cluster.

Read more about Minikube from the official [Minikube documentation](https://minikube.sigs.k8s.io/docs/), the official [Kubernetes documentation](https://kubernetes.io/docs/setup/learning-environment/minikube/), or [GitHub](https://github.com/kubernetes/minikube).

### Installing Minikube on Linux ###

Let's learn how to install the latest Minikube release on Ubuntu Linux 18.04 LTS with VirtualBox v6.1 specifically.

NOTE: For other Linux OS distributions or releases, VirtualBox and Minikube versions the installation steps may vary! Check the [Minikube installation!](https://kubernetes.io/docs/tasks/tools/install-minikube/)

Verify the virtualization support on your Linux OS (a non-empty output indicates supported virtualization):

    $ grep -E --color 'vmx|svm' /proc/cpuinfo

Install the [VirtualBox](https://www.virtualbox.org/wiki/Linux_Downloads) hypervisor
Add the source repository for the bionic distribution (Ubuntu 18.04), download and register the public key, update and install:

    $ sudo bash -c 'echo "deb [arch=amd64] https://download.virtualbox.org/virtualbox/debian bionic contrib" >> /etc/apt/sources.list'

    $ wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -

    $ sudo apt update

    $ sudo apt install -y virtualbox-6.1

Install Minikube
We can download the latest release or a specific release from the [Minikube release page](https://github.com/kubernetes/minikube/releases). Once downloaded, we need to make it executable and add it to our PATH:

    $ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

    NOTE: Replacing /latest/ with a particular version, such as /v1.13.0/ will download that specified version. 

Start Minikube
We can start Minikube with the minikube start command, that bootstraps a single-node cluster with the latest stable Kubernetes version release. For a specific Kubernetes version the --kubernetes-version option can be used as such minikube start --kubernetes-version v1.19.0 (where latest is default and acceptable version value, and stable is also acceptable):

    $ minikube start

Check the status

With the minikube status command, we display the status of Minikube:

    $ minikube status

Stop Minikube
With the minikube stop command, we can stop Minikube:

    $ minikube stop

### Installing Minikube on macOS ###

Let's learn how to install the latest Minikube release on Mac OS X with VirtualBox v6.1 specifically.

NOTE: For other VirtualBox and Minikube versions the installation steps may vary! Check the [Minikube installation!](https://kubernetes.io/docs/tasks/tools/install-minikube/O)

Verify the virtualization support on your macOS (VMX in the output indicates enabled virtualization):

    $ sysctl -a | grep -E --color 'machdep.cpu.features|VMX'

Although VirtualBox is the default hypervisor for Minikube, on Mac OS X we can configure Minikube at startup to use another hypervisor (downloaded separately), with the --driver=parallels or --driver=hyperkit start option.

Install the [VirtualBox](https://www.virtualbox.org/wiki/Downloads) hypervisor for 'OS X hosts'
Download and install the .dmg package.

Install Minikube
We can download the latest release or a specific release from the Minikube release page. Once downloaded, we need to make it executable and add it to our PATH:

    $ curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/

    NOTE: Replacing /latest/ with a particular version, such as /v1.13.0/ will download that specified version.

Start Minikube

We can start Minikube with the minikube start command, that bootstraps a single-node cluster with the latest stable Kubernetes version release. For a specific Kubernetes version the --kubernetes-version option can be used as such minikube start --kubernetes-version v1.19.0 (where latest is default and acceptable version value, and stable is also acceptable):

    $ minikube start

Check the status
With the minikube status command, we display the status of Minikube:

    $ minikube status

Stop Minikube
With the minikube stop command, we can stop Minikube:

    $ minikube stop

### Installing Minikube on Windows ###

Let's learn how to install the latest Minikube release on Windows 10 with VirtualBox v6.1 specifically.

NOTE: For other OS, VirtualBox, and Minikube versions, the installation steps may vary! Check the Minikube installation!

Verify the virtualization support on your Windows system (multiple output lines ending with 'Yes' indicate supported virtualization):

    PS C:\WINDOWS\system32> systeminfo

Install the VirtualBox hypervisor for 'Windows hosts'
Download and install the .exe package.

NOTE: You may need to disable Hyper-V on your Windows host (if previously installed and used) while running VirtualBox.

Install Minikube
We can download the latest release or a specific release from the Minikube release page. Once downloaded, we need to make sure it is added to our PATH.

There are two .exe packages available to download for Windows found under a Minikube release:
* minikube-windows-amd64.exe which requires to be added to the PATH: manually
* minikube-installer.exe which automatically adds the executable to the PATH. 

Let's download and install the latest minikube-installer.exe package. 

Start Minikube
We can start Minikube using the minikube start command, that bootstraps a single-node cluster with the latest stable Kubernetes version release. For a specific Kubernetes version the --kubernetes-version option can be used as such minikube start --kubernetes-version v1.19.0 (where latest is default and acceptable version value, and stable is also acceptable). Open the PowerShell using the Run as Administrator option and execute the following command:

    PS C:\WINDOWS\system32> minikube start

Check the status
We can see the status of Minikube using the minikube status command. Open the PowerShell using the Run as Administrator option and execute the following command:

    PS C:\WINDOWS\system32> minikube status 

Stop Minikube
We can stop Minikube using the minikube stop command. Open the PowerShell using the Run as Administrator option and execute the following command:

    PS C:\WINDOWS\system32> minikube stop 

### Minikube CRI-O ###

According to the CRI-O website,

"CRI-O is an implementation of the Kubernetes CRI (Container Runtime Interface) to enable using OCI (Open Container Initiative) compatible runtimes."

Start Minikube with CRI-O as container runtime, instead of Docker, with the following command:

NOTE: While docker is the default runtime, minikube Kubernetes also supports cri-o and containerd. 

    $ minikube start --container-runtime cri-o

By describing a running Kubernetes pod, we can extract the Container ID field of the pod that includes the name of the runtime:

    $ kubectl -n kube-system describe pod kube-scheduler-minikube | grep "Container ID"

Let's login via ssh into the Minikube's VM:

    $ minikube ssh

NOTE: If you try to list containers using the docker command, it will not produce any results, because Docker is not running our containers at this time:

    $ sudo docker container ls
    Cannot connect to the Docker daemon at unix:///var/run/docker.sock. Is the docker daemon running?

List the containers created by the CRI-O container runtime and extract the container manager from the config file:

    $ sudo runc list 

    $ sudo cat /run/containers/storage/overlay-containers

## Learning Objectives (Review) ##

You should now be able to:

* Understand Minikube.
* Install Minikube on local Linux, macOS, and Windows workstation.
* Verify the local installation.

