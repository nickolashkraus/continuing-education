# Chapter 7: Accessing Minikube

## Tasks
- [x] None

## Introduction

In this chapter, we will learn about different methods of accessing a Kubernetes cluster.

We can use a variety of external clients or custom scripts to access our cluster for administration purposes. We will explore `kubectl` as a CLI tool to access the Minikube Kubernetes cluster, the **Kubernetes Dashboard** as a web-based user interface to interact with the cluster, and the `curl` command with proper credentials to access the cluster via APIs.

## Accessing Minikube: Command Line Interface (CLI)

[`kubectl`](https://kubernetes.io/docs/reference/kubectl/overview) is the **Kubernetes Command Line Interface (CLI) client** to manage cluster resources and applications. It is very flexible and easy to integrate with other systems, therefore it can be used standalone, or part of scripts and automation tools. Once all required credentials and cluster access points have been configured for `kubectl`, it can be used remotely from anywhere to access a cluster.

## Accessing Minikube: Web-based User Interface (Web UI)

The [Kubernetes Dashboard](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard) provides a **Web-Based User Interface (Web UI)** to interact with a Kubernetes cluster to manage resources and containerized applications. In one of the later chapters, we will be using it to deploy a containerized application.

## Accessing Minikube: APIs

The main component of the Kubernetes control plane is the **API server**, responsible for exposing the Kubernetes APIs. The APIs allow operators and users to directly interact with the cluster. Using both CLI tools and the Dashboard UI, we can access the API server running on the master node to perform various operations to modify the cluster's state. The API server is accessible through its endpoints by agents and users possessing the required credentials.

The HTTP API directory tree of Kubernetes can be divided into three independent group types:

**Core Group** (`/api/v1`)
* This group includes objects such as Pods, Services, Nodes, Namespaces, ConfigMaps, Secrets, etc.

**Named Group**
* This group includes objects in `/apis/$NAME/$VERSION` format. These different API versions imply different levels of stability and support:
  * *Alpha level*: It may be dropped at any point in time, without notice. For example, `/apis/batch/v2alpha1`.
  * *Beta level*: It is well-tested, but the semantics of objects may change in incompatible ways in a subsequent beta or stable release. For example, `/apis/certificates.k8s.io/v1beta1`.
	* *Stable level*: Appears in released software for many subsequent versions. For example, `/apis/networking.k8s.io/v1`.

**System-wide**
* This group consists of system-wide API endpoints, like `/healthz`, `/logs`, `/metrics`, `/ui`, etc.

We can access an API server either directly by calling the respective API endpoints, using the CLI tools, or the Dashboard UI.

## kubectl

`kubectl` allows us to manage local Kubernetes clusters like the Minikube cluster, or remote clusters deployed in the cloud. It is generally installed before installing and starting Minikube, but it can also be installed after the cluster bootstrapping step. Once installed, `kubectl` receives its configuration automatically for Minikube Kubernetes cluster access. However, in different Kubernetes cluster setups, we may need to manually configure the cluster access points and certificates required by `kubectl` to securely access the cluster.

There are different methods that can be used to install `kubectl` listed in the [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl). For best results, it is recommended to keep `kubectl` at the same version with the Kubernetes run by Minikube.

Details about the `kubectl` command line client can be found in the [kubectl book](https://kubectl.docs.kubernetes.io), the [Kubernetes official documentation](https://kubernetes.io/search/?q=kubectl), or its [GitHub repository](https://github.com/kubernetes/kubectl).

## kubectl Configuration File

To access the Kubernetes cluster, the `kubectl` client needs the master node endpoint and appropriate credentials to be able to securely interact with the API server running on the master node. While starting Minikube, the startup process creates, by default, a configuration file, `config`, inside the `.kube` directory (often referred to as the [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig)), which resides in the user's **home** directory. The configuration file has all the connection details required by `kubectl`. By default, the `kubectl` binary parses this file to find the master node's connection endpoint, along with credentials. Multiple kubeconfig files can be configured with a single `kubectl` client. To look at the connection details, we can either display the content of the `~/.kube/config` file (on Linux) or run the following command:

```bash
$ kubectl config view
```

The kubeconfig includes the API server's endpoint **server: https://192.168.99.100:8443** and the **minikube** user's client authentication **key** and **certificate** data.

Once `kubectl` is installed, we can display information about the Minikube Kubernetes cluster with the **kubectl cluster-info** command:

```bash
$ kubectl cluster-info
```

You can find more details about the `kubectl` command line options [here](https://kubernetes.io/docs/reference/kubectl/overview).

Although for the Kubernetes cluster installed by Minikube the `~/.kube/config` file gets created automatically, this is not the case for Kubernetes clusters installed by other tools. In other cases, the config file has to be created manually and sometimes re-configured to suit various networking and client/server setups.
