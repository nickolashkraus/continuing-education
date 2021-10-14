# Chapter 4: Containers

## Tasks
- [ ] Read [Open Container Initiative Runtime Specification](https://github.com/opencontainers/runtime-spec)
- [ ] Read [Open Container Image Format Specification](https://github.com/opencontainers/image-spec)
- [ ] Read [Introducing Container Runtime Interface (CRI) in Kubernetes](https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes)
- [ ] Read [Docker overview](https://docs.docker.com/get-started/overview/#docker-architecture)

## Introduction

When multiple applications are deployed on one host, developers are faced with a challenge - how to isolate applications from one another to avoid conflicts between dependencies, libraries, and runtimes.

[Operating-System-level virtualization](https://en.wikipedia.org/wiki/Operating-system-level_virtualization) allows us to run multiple isolated user-space instances in parallel. These user-space instances include the application source code, required libraries, and the required runtime to run the application without any external dependencies. These user-space instances are referred to as **containers**. They solve the dependencies, libraries, and runtimes conflict challenges developers have been facing in the past.

From the Wikipedia article on OS-level virtualization,

>"OS-level virtualization is an operating system paradigm in which the kernel allows the existence of multiple isolated user space instances. Such instances, called containers (LXC, Solaris containers, Docker), Zones (Solaris containers), virtual private servers (OpenVZ), partitions, virtual environments (VEs), virtual kernels (DragonFly BSD), or jails (FreeBSD jail or chroot jail), may look like real computers from the point of view of programs running in them. A computer program running on an ordinary operating system can see all resources (connected devices, files and folders, network shares, CPU power, quantifiable hardware capabilities) of that computer. However, programs running inside of a container can only see the container's contents and devices assigned to the container."

## Images and Containers

In the container world, this box containing our application source code and all its dependencies and libraries is referred to as an **image**. A running instance of this box is referred to as a **container**. We can run multiple containers from the same image.

An image contains the application, its dependencies, and the user-space libraries. User-space libraries like **glibc** enable switching from the user-space to the kernel-space. An image does not contain any kernel-space components.

When a container is created from an image, it runs as a process on the host's kernel. It is the host kernel's job to isolate the container process and to provide resources to each container.

## The Container Technology: Building Blocks

We will now take a look at some of the building blocks of the container technology, provided by the Linux kernel.

**Namespaces**
A namespace wraps a particular global system resource like network, process IDs in an abstraction, that makes it appear to the processes within the namespace that they have their own isolated instance of the global resource. The following global resources are namespaced:

* pid - provides each namespace to have the same PIDs. Each container has its own PID 1.
* net - allows each namespace to have its own network stack. Each container has its own IP address.
* mnt - allows each namespace to have its own view of the filesystem hierarchy.
* ipc - allows each namespace to have its own interprocess communication.
* uts - allows each namespace to have its own hostname and domain name.
* user - allows each namespace to have its own user and group ID number spaces. A *root* user inside a container is not the *root* user of the host on which the container is running.

**cgroups**
*Control groups* are used to organize processes hierarchically and distribute system resources along the hierarchy in a controlled and configurable manner. The following cgroups are available for Linux:

* blkio
* cpu
* cpuacct
* cpuset
* devices
* freezer
* memory

**Union filesystem**
The Union filesystem allows files and directories of separate filesystems, known as layers, to be transparently overlaid on top of each other, to create a new virtual filesystem. An image used in Docker is made of multiple layers and, while starting a new container, we merge all those layers to create a read-only filesystem. On top of a read-only filesystem, a container gets a read-write layer, which is an ephemeral layer and it is local to the container.

## Container Runtimes

Namespaces and cgroups have existed in the Linux kernel for quite some time, but consuming them to create containers was not easy. After Docker was launched in 2013, containers started to become mainstream. Docker hid all the complexities in the background and came up with an easy workflow to share and manage both images and containers.

Docker achieved this level of simplicity through a collection of tools that interact with a container runtime on behalf of the user. The container runtime ensures containers portability, offering a consistent environment for containers to run, regardless of the infrastructure. Some of the container runtimes are provided below:

[**runC**](https://github.com/opencontainers/runc)
Over the past few years, we have seen rapid growth in the interest and adoption of container technologies. Most of the cloud providers and IT vendors offer support for containers. To make sure there is no vendor locking and no inclination towards a particular company or project, IT companies came together and formed an open governance structure, called The [Open Container Initiative](https://www.opencontainers.org) (OCI), under the auspices of The Linux Foundation. The governance body came up with both [runtime](https://github.com/opencontainers/runtime-spec) and [image](https://github.com/opencontainers/image-spec) specifications to create standards on the Operating System process and application containers. runC is the CLI tool for spawning and running containers according to these specifications.

[**containerd**](https://containerd.io)
containerd is an OCI-compliant container runtime with an emphasis on simplicity, robustness, and portability. It runs as a daemon and manages the entire lifecycle of containers. It is available on Linux and Windows. Docker, which is a containerization platform, uses containerd as a container runtime to manage runC containers.

[**rkt**](https://github.com/rkt/rkt)
rkt (pronounced "rock-it") is an open source, Apache 2.0-licensed project from CoreOS. It implements the App Container specification.

[**CRI-O**](http://cri-o.io)
CRI-O is an OCI-compatible runtime, which is an implementation of the [Kubernetes Container Runtime Interface](https://kubernetes.io/blog/2016/12/container-runtime-interface-cri-in-kubernetes) (CRI). It is a lightweight alternative to using Docker as the runtime for Kubernetes.

## Containers vs. VMs

A virtual machine runs on top of a [hypervisor](https://en.wikipedia.org/wiki/Hypervisor), which virtualizes a host system's hardware by emulating CPU, memory and networking hardware resources so that a guest OS can be installed on top of them. Different kinds of guest OSes can run on top of one hypervisor. Between an application running inside a guest OS and in the outside world, there are multiple layers: the guest OS, the hypervisor, and at times the host OS.

![Docker Container vs. VMs](./img/img_0.png)

In contrast to VMs, containers run directly as processes on the host OS. There is no indirection as we see in VMs, which helps containers to achieve near-native performance. Also, as the containers have a very light footprint, it becomes easier to pack a higher number of containers than VMs on the same physical machine. However, as containers run on the host OS, we need to make sure containers are compatible with the host OS.

## Docker

Docker has a client-server [architecture](https://docs.docker.com/get-started/overview/#docker-architecture), with a Docker Client connecting to a Docker Host server to execute commands.
