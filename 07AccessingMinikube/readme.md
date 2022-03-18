# Chapter 7. Accessing Minikube #

## Introduction and Learning Objectives ##

### Introduction ###

In this chapter, we will learn about different methods of accessing a Kubernetes cluster.

We can use a variety of external clients or custom scripts to access our cluster for administration purposes. We will explore kubectl as a CLI tool to access the Minikube Kubernetes cluster, the Kubernetes Dashboard as a web-based user interface to interact with the cluster, and the curl command with proper credentials to access the cluster via APIs.

### Learning Objectives ###

By the end of this chapter, you should be able to:

* Compare methods to access a Kubernetes cluster.
* Configure kubectl for Linux, macOS, and Windows.
* Access the Minikube Kubernetes cluster from the Dashboard.
* Access Minikube Kubernetes cluster via APIs.

## Accessing Minikube ##

### Accessing Minikube ###

Any healthy running Kubernetes cluster can be accessed via any one of the following methods:

* Command Line Interface (CLI) tools and scripts
* Web-based User Interface (Web UI) from a web browser
* APIs from CLI or programmatically

These methods are applicable to all Kubernetes clusters. 

### Accessing Minikube: Command Line Interface (CLI) ###

kubectl is the Kubernetes Command Line Interface (CLI) client to manage cluster resources and applications. It is very flexible and easy to integrate with other systems, therefore it can be used standalone, or part of scripts and automation tools. Once all required credentials and cluster access points have been configured for kubectl, it can be used remotely from anywhere to access a cluster. 

In later chapters, we will be using kubectl to deploy applications, manage and configure Kubernetes resources.

### Accessing Minikube: Web-based User Interface (Web UI) ###

The Kubernetes Dashboard provides a Web-Based User Interface (Web UI) to interact with a Kubernetes cluster to manage resources and containerized applications. In one of the later chapters, we will be using it to deploy a containerized application.

### Accessing Minikube: APIs ###

The main component of the Kubernetes control plane is the API server, responsible for exposing the Kubernetes APIs. The APIs allow operators and users to directly interact with the cluster. Using both CLI tools and the Dashboard UI, we can access the API server running on the master node to perform various operations to modify the cluster's state. The API server is accessible through its endpoints by agents and users possessing the required credentials.

Below, we can see the representation of the HTTP API directory tree of Kubernetes: 

![api diag](https://courses.edx.org/assets/courseware/v1/7ebcb514203a3af89dc1599625779c1f/asset-v1:LinuxFoundationX+LFS158x+3T2020+type@asset+block/api-server-space_.jpg)

HTTP API directory tree of Kubernetes can be divided into three independent group types:

* Core Group (/api/v1)
  This group includes objects such as Pods, Services, Nodes, Namespaces, ConfigMaps, Secrets, etc.

* Named Group
  This group includes objects in /apis/$NAME/$VERSION format. These different API versions imply different levels of stability and support:
  Alpha level - it may be dropped at any point in time, without notice. For example, /apis/batch/v2alpha1.
  Beta level - it is well-tested, but the semantics of objects may change in incompatible ways in a subsequent beta or stable release. For example, /apis/certificates.k8s.io/v1beta1.
  Stable level - appears in released software for many subsequent versions. For example, /apis/networking.k8s.io/v1.

* System-wide
  This group consists of system-wide API endpoints, like /healthz, /logs, /metrics, /ui, etc.

We can access an API server either directly by calling the respective API endpoints, using the CLI tools, or the Dashboard UI.

Next, we will see how we can access the Minikube Kubernetes cluster we set up in the previous chapter.

### kubectl ###

kubectl allows us to manage local Kubernetes clusters like the Minikube cluster, or remote clusters deployed in the cloud. It is generally installed before installing and starting Minikube, but it can also be installed after the cluster bootstrapping step. Once installed, kubectl receives its configuration automatically for Minikube Kubernetes cluster access. However, in different Kubernetes cluster setups, we may need to manually configure the cluster access points and certificates required by kubectl to securely access the cluster.

There are different methods that can be used to install kubectl listed in the [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/). For best results, it is recommended to keep kubectl at the same version with the Kubernetes run by Minikube. Next, we will look at a few steps to install it on Linux, macOS, and Windows systems.

Details about the kubectl command line client can be found in the [kubectl book](https://kubectl.docs.kubernetes.io/), the [Kubernetes official documentation](https://kubernetes.io/search/?q=kubectl), or its [github repository](https://github.com/kubernetes/kubectl).

### Installing kubectl on Linux ###

To install kubectl on Linux, follow the instruction below:

Download the latest stable kubectl binary, make it executable and move it to the PATH:

    $ curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl" && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

Where https://storage.googleapis.com/kubernetes-release/release/stable.txt aims to display the latest Kubernetes stable release version.

NOTE: To download and setup a specific version of kubectl (such as v1.19.1), issue the following command:

    $ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.19.1/bin/linux/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

A typical helpful post-installation configuration is to enable shell autocompletion for kubectl. It can be achieved by running the following four commands: 

    $ sudo apt install -y bash-completion
    $ source /usr/share/bash-completion/bash_completion
    $ source <(kubectl completion bash)
    $ echo 'source <(kubectl completion bash)' >>~/.bashrc

### Installing kubectl on macOS ###

There are two methods to install kubectl on macOS: manually and using the Homebrew package manager. Next, we present both installation methods.

To manually install kubectl, download the latest stable kubectl binary, make it executable and move it to the PATH with the following command:

    $ curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/darwin/amd64/kubectl" && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

Where https://storage.googleapis.com/kubernetes-release/release/stable.txt aims to display the latest Kubernetes stable release version.

NOTE: To download and setup a specific version of kubectl (such as v1.19.1), issue the following command:

    $ curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.19.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/

To install kubectl with Homebrew package manager, issue the following command:

    $ brew install kubernetes

or

    $ brew install kubernetes-cli

A typical helpful post-installation configuration is to enable [shell autocompletion](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion) for kubectl.

### Installing kubectl on Windows ###

To install kubectl, we can download the binary directly or use curl from the CLI. Once downloaded the binary needs to be added to the PATH.

Direct download link for v1.19.2 binary (just click below):

https://storage.googleapis.com/kubernetes-release/release/v1.19.2/bin/windows/amd64/kubectl.exe

NOTE: Obtain the latest kubectl stable release version number from the link below, and if needed, edit the download link for the desired binary version from above: 

https://storage.googleapis.com/kubernetes-release/release/stable.txt

Use the curl command (if installed) from the CLI:

    curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.19.2/bin/windows/amd64/kubectl.exe

Once downloaded, move the kubectl binary to the PATH.

NOTE: Docker Desktop for Windows adds its own version of kubectl to PATH. If you have installed Docker Desktop before, you may need to place your PATH entry before the one added by the Docker Desktop installer or remove the Docker Desktop's kubectl.

### kubectl Configuration File ###

To access the Kubernetes cluster, the kubectl client needs the master node endpoint and appropriate credentials to be able to securely interact with the API server running on the master node. While starting Minikube, the startup process creates, by default, a configuration file, config, inside the.kube directory (often referred to as the kubeconfig), which resides in the user's home directory. The configuration file has all the connection details required by kubectl. By default, the kubectl binary parses this file to find the master node's connection endpoint, along with credentials. Multiple kubeconfig files can be configured with a single kubectl client. To look at the connection details, we can either display the content of the ~/.kube/config file (on Linux) or run the following command: 

    $ kubectl config view

The kubeconfig includes the API server's endpoint server: https://192.168.99.100:8443 and the minikube user's client authentication key and certificate data.

Once kubectl is installed, we can display information about the Minikube Kubernetes cluster with the kubectl cluster-info command: 

    $ kubectl cluster-info

You can find more details about the kubectl command line options [here](https://kubernetes.io/docs/reference/kubectl/overview/).

Although for the Kubernetes cluster installed by Minikube the ~/.kube/config file gets created automatically, this is not the case for Kubernetes clusters installed by other tools. In other cases, the config file has to be created manually and sometimes re-configured to suit various networking and client/server setups.

### Kubernetes Dashboard ###

As mentioned earlier, the Kubernetes Dashboard provides a web-based user interface for Kubernetes cluster management. To access the dashboard from Minikube, we can use the minikube dashboard command, which opens a new tab in our web browser, displaying the Kubernetes Dashboard:

    minikube dashboard 

NOTE: In case the browser is not opening another tab and does not display the Dashboard as expected, verify the output in your terminal as it may display a link for the Dashboard (together with some Error messages). Copy and paste that link in a new tab of your browser. Depending on your terminal's features you may be able to just click or right-click the link to open directly in the browser. The link may look similar to:

http://localhost:37751/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/

Chances are that the only difference is the PORT number 37751 above, while your port number may be different.

After a logout/login or a reboot of your workstation the normal behavior should be expected (where the minikube dashboard command directly opens a new tab in your browser displaying the Dashboard).

### Kubernetes Dashboard with 'kubectl proxy' ###

Issuing the kubectl proxy command, kubectl authenticates with the API server on the master node and makes the Dashboard available on a slightly different URL than the one earlier, this time through the default proxy port 8001.

First, we issue the kubectl proxy command:

    $ kubectl proxy

It locks the terminal for as long as the proxy is running, unless we run it in the background (with kubectl proxy &). With the proxy running we can access the Dashboard over the new URL (just click on it below - it should work on your workstation). Once we stop the proxy (with CTRL + C) the Dashboard URL is no longer accessible.

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ 

### APIs with 'kubectl proxy' ###

When kubectl proxy is running, we can send requests to the API over the localhost on the default proxy port 8001 (from another terminal, since the proxy locks the first terminal when running in foreground):

    $ curl http://localhost:8001/

With the above curl request, we requested all the API endpoints from the API server. Clicking on the link above (in the curl command), it will open the same listing output in a browser tab.

We can explore several path combinations with curl or in a browser as well, such as:

    http://localhost:8001/api/v1
    http://localhost:8001/apis/apps/v1
    http://localhost:8001/healthz
    http://localhost:8001/metrics

### APIs with Authentication ###

When not using the kubectl proxy, we need to authenticate to the API server when sending API requests. We can authenticate by providing a Bearer Token when issuing a curl, or by providing a set of keys and certificates.

A Bearer Token is an access token which is generated by the authentication server (the API server on the master node) and given back to the client. Using that token, the client can connect back to the Kubernetes API server without providing further authentication details, and then, access resources.

Get the token:

    $ TOKEN=$(kubectl describe secret -n kube-system $(kubectl get secrets -n kube-system | grep default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d '\t' | tr -d " ")

Get the API server endpoint:

    $ APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")

Confirm that the APISERVER stored the same IP as the Kubernetes master IP by issuing the following 2 commands and comparing their outputs:

    $ echo $APISERVER

    $ kubectl cluster-info

Access the API server using the curl command, as shown below:

    $ curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure

Instead of the access token, we can extract the client certificate, client key, and certificate authority data from the .kube/config file. Once extracted, they can be encoded and then passed with a curl command for authentication. The new curl command would look similar to the example below. Keep in mind, however, that the below example command would only work with the encoded client certificae, key and certificate authority data.

    $ curl $APISERVER --cert encoded-cert --key encoded-key --cacert encoded-ca

### Learning Objectives (Review) ###

You should now be able to:

* Compare methods to access a Kubernetes cluster.
* Configure kubectl for Linux, macOS, and Windows.
* Access the Minikube Kubernetes cluster from the Dashboard.
* Access Minikube Kubernetes via APIs.

