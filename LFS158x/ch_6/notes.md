# Chapter 6: Minikube - A Local Kubernetes Cluster

## Tasks
- [x] None

## Introduction

As mentioned earlier, [Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube) is the easiest and most popular method to run an all-in-one Kubernetes cluster in a virtual machine (VM) locally on our workstations. Minikube is the tool responsible for the installation of Kubernetes components, cluster bootstrapping, and cluster tear-down when no longer needed. It includes a few additional tools aimed to ease the user interaction with the Kubernetes cluster, but nonetheless, it initializes for us an all-in-one single-node Kubernetes cluster. While the latest Minikube release also supports multi-node Kubernetes clusters, this feature is still experimental.

In this chapter, we will explore the requirements to install Minikube locally on our workstation, together with the installation instructions to set it up on local Linux, macOS, and Windows operating systems.

## Requirements for Running Minikube

Minikube is installed and runs directly on a local Linux, macOS, or Windows workstation. However, in order to fully take advantage of all the features Minikube has to offer, a [Type-2 Hypervisor](https://en.wikipedia.org/wiki/Hypervisor) or a Container Runtime should be installed on the local workstation, to run in conjunction with Minikube. This does not mean that we need to create any VMs with guest operating systems with this Hypervisor or Containers. Minikube builds all its infrastructure as long as the Type-2 Hypervisor, or a Container Runtime, is installed on our workstation.

Minikube uses [libmachine](https://github.com/docker/machine/tree/master/libmachine) to invoke the Hypervisor that provisions the VM which hosts a single-node Kubernetes cluster, or the Container Runtime to run the Container that hosts the cluster. Minikube then uses [kubeadm](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm) to provision the Kubernetes cluster. Although VirtualBox is Minikube's original hypervisor driver, several other drivers are supported by Minikube when provisioning the environment in which the Kubernetes cluster is to be bootstrapped. Thus we need to make sure that we have the necessary hardware and software required by Minikube to build our environment.

Below we outline the requirements to run Minikube on our local workstation:
* VT-x/AMD-v virtualization must be enabled on the local workstation in BIOS, and/or [verify](https://kubernetes.io/docs/tasks/tools/install-minikube/#before-you-begin) if virtualization is supported by your workstation's OS.
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl) is a binary used to access and manage any Kubernetes cluster. It is installed through Minikube and accessed through the **minikube** command, or it can be installed separately. We will explore **kubectl** in more detail in future chapters.
* Type-2 Hypervisor or Container Runtime
		* On Linux [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [KVM2](https://www.linux-kvm.org/page/Main_Page) or Docker and Podman runtimes
		* On macOS [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [HyperKit](https://github.com/moby/hyperkit), [VMware Fusion](http://www.vmware.com/products/fusion.html), [Parallels](https://minikube.sigs.k8s.io/docs/drivers/parallels) or Docker runtime
		* On Windows [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v), or Docker runtime.

**NOTE**: Minikube supports a [--driver=none](https://minikube.sigs.k8s.io/docs/drivers/none) (on Linux) option that runs the Kubernetes components directly on the host OS and not inside a VM. With this option a Docker installation is required and a Linux OS on the local workstation, but no hypervisor installation. If you use **—driver=none** in Debian or its derivatives, be sure to download [.deb Docker packages](https://www.docker.com/products/docker-desktop) instead of the snap package which does not work with Minikube. In addition to hypervisors, Minikube also supports a limited number of container runtimes, with the [—driver=docker](https://minikube.sigs.k8s.io/docs/drivers/docker) (on Linux, macOS, and Windows) and [—driver=podman](https://minikube.sigs.k8s.io/docs/drivers/podman) (on Linux) options, to install and run the Kubernetes cluster on top of a container runtime.

* Internet connection at least on first Minikube run - to download packages, dependencies, updates and pull images needed to initialize the Minikube Kubernetes cluster. Subsequent runs will require an internet connection only when new Docker images need to be pulled from a container registry or when deployed containerized applications need it. Once an image has been pulled it can be reused without an internet connection.

Read more about Minikube from the official [Minikube documentation](https://minikube.sigs.k8s.io/docs), the official [Kubernetes documentation](https://kubernetes.io/docs/setup/learning-environment/minikube), or [GitHub](https://github.com/kubernetes/minikube).

## Installing Minikube on Linux

Verify the virtualization support on your Linux OS (a non-empty output indicates supported virtualization):

```bash
grep -E --color 'vmx|svm' /proc/cpuinfo
```

See the [documentation](https://minikube.sigs.k8s.io/docs) for installation details.

## Installing Minikube on macOS

Verify the virtualization support on your macOS (VMX in the output indicates enabled virtualization):

```bash
sysctl -a | grep -E --color 'machdep.cpu.features|VMX'
```

See the [documentation](https://minikube.sigs.k8s.io/docs) for installation details.

## Installing Minikube on Windows

Verify the virtualization support on your Windows system (multiple output lines ending with 'Yes' indicate supported virtualization):

```
PS C:\WINDOWS\system32> systeminfo
```

See the [documentation](https://minikube.sigs.k8s.io/docs) for installation details.

## Minikube CRI-O

According to the [CRI-O website](http://cri-o.io/),

>"CRI-O is an implementation of the Kubernetes CRI (Container Runtime Interface) to enable using OCI (Open Container Initiative) compatible runtimes."

Start Minikube with CRI-O as container runtime, instead of Docker, with the following command:

```bash
minikube start --container-runtime cri-o
```

**NOTE**: While **docker** is the default runtime, minikube Kubernetes also supports **cri-o** and **containerd**.
