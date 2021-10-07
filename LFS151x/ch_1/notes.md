# Chapter 1: Virtualization

## Tasks
- [x] Add brief description of OpenStack
- [x] Add brief description of a hypervisor

## Introduction to Cloud Computing and Technologies

Cloud computing can be referred to as the remote allocation of resources on the cloud. According to [NIST (National Institute of Standard and Technology)](http://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-145.pdf), the formal definition of cloud computing is the following:

>"Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (e.g. networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction."

## Key Features of Cloud Computing

Users benefit most from the following features of cloud computing:

**Speed and Agility**
* Cloud resources are provisioned with a few clicks, saving time and providing agility. They also scale up or down with ease, by increasing or decreasing the number of running instances of a particular resource as a response to current demand or based on projected future demand. An additional characteristic is elasticity, allowing some cloud resources instances to increase or decrease in size, also demand-driven.

**Cost**
* By reducing the up-front cost to set up the infrastructure, it allows users to focus on applications supporting the business processes. Another beneficial feature of cloud providers is a cost estimator, which assists users in better resource planning.

**Easy Access to Resources**
* Users can access the cloud services from any place and device, as long as there is connectivity to the cloud service provider.

**Maintenance**
* All the lights-on maintenance work to keep resources up, running, and up-to-date is offloaded onto the provider, and it is no longer the user's concern.

**Multi-tenancy**
* Multiple users sharing the same pool of resources, which helps to drive down the costs incurred by each user.

**Reliability**
* Resources can be hosted in different data center locations, to provide increased reliability, resiliency, and high-availability.

## Cloud Deployment Models

Primarily, a cloud is deployed under the following models:

**Private Cloud**
* It is designated and operated solely for one organization. It can be hosted internally or externally and managed by internal teams or a third party. A private cloud can be built using a software stack like [OpenStack](https://www.openstack.org).

From the OpenStack [documentation](https://www.openstack.org/software),

>"OpenStack is a cloud operating system that controls large pools of compute, storage, and networking resources throughout a datacenter, all managed and provisioned through APIs with common authentication mechanisms."

**Public Cloud**
* It is open to the public and anybody can use it, after swiping a credit card, of course. Amazon Web Services, Google Cloud Platform, and Microsoft Azure are examples of public clouds.

**Hybrid Cloud**
* Public and private clouds can be bound together to offer a hybrid cloud. Such hybrid clouds are beneficial for:

1. Storage of sensitive information on the private cloud, while offering public services based on that information from a public cloud.
2. Meeting temporary resources during peak or times of high demand by scaling to the cloud, when such temporary resources cannot be met on the private cloud.

## Virtualization

According to [Wikipedia](https://en.wikipedia.org/wiki/Virtualization),

>"In computing, virtualization refers to the act of creating a virtual (rather than actual) version of something, including virtual computer hardware platforms, operating systems, storage devices, and computer resources."

Virtualization can be achieved at different hardware and software layers, such as the Central Processing Unit (CPU), storage (disk), memory (RAM), filesystems, etc. In this chapter, we will explore a few tools that allow us to create Virtual Machines (VMs) after emulating essential hardware components that support the installation of a guest Operating System (OS) on them. A VM represents an isolated collection of emulated resources, behaving like an actual physical system.

Virtual Machines are created with the help of a [hypervisor](https://en.wikipedia.org/wiki/Hypervisor), that runs on a host machine. The hypervisor is a piece of software capable of creating multiple isolated virtual operating environments, each composed of emulated resources that are then made available to guest systems. Hypervisors are classified into two main categories:

From Wikipedia,

>"A hypervisor (or virtual machine monitor, VMM, virtualizer) is a kind of emulator; it is computer software, firmware or hardware that creates and runs virtual machines."

**Type-1 hypervisor** (native or bare-metal) runs directly on top of a physical host machine's hardware, without the need for a host OS. Typically, they are found in enterprise settings. Examples of type-1 hypervisors:
* IBM z/VM
* Microsoft Hyper-V
* Nutanix AHV
* Oracle VM Server for SPARC
* Oracle VM Server for x86
* VMware ESXi
* AWS Nitro
* Xen

**Type-2 hypervisor** (hosted) runs on top of the host's OS. Typically, they are for end-users, but they may be found in enterprise settings as well. Examples of type-2 hypervisors:
* VirtualBox
* VMware Player
* VMware Workstation
* Parallels Desktop for Mac

And then there are the exceptions - hypervisors matching both categories, which can be listed redundantly under hypervisors of both type-1 and type-2. They are Linux kernel modules that act as both type-1 and type-2 hypervisors at the same time. Examples are:
* KVM
* bhyve

Hypervisors enable the virtualization of computer hardware such as CPU, disk, network, RAM, etc., and allow the installation of guest VMs on top of them. We can create multiple guest VMs with different Operating Systems on a single hypervisor. For example, we can use a native Linux machine as a host, running on bare-metal, and after setting up a type-2 hypervisor we can create multiple guest machines with different OSes, commonly Linux and Windows.

Hardware virtualization is supported by most modern CPUs, and it is the feature that allows hypervisors to virtualize the physical hardware of a host system, thus sharing the host system's processing resources with multiple guest systems in a safe and efficient manner. In addition, most modern CPUs also support [nested virtualization](https://en.wikipedia.org/wiki/Virtualization#Nested_virtualization), which allows us to have VMs inside a VM.

## Introduction to KVM

According to [linux-kvm.org](https://www.linux-kvm.org),

>"KVM (for Kernel-based Virtual Machine) is a full virtualization solution for Linux on x86 hardware containing virtualization extensions (Intel VT or AMD-V). It consists of a loadable kernel module, kvm.ko, that provides the core virtualization infrastructure and a processor specific module, kvm-intel.ko or kvm-amd.ko."

KVM is a loadable virtualization module of the Linux kernel and it converts the kernel into a hypervisor capable of managing guest Virtual Machines. Specific hardware virtualization extensions supported by most modern processors, such as Intel VT and AMD-V, have to be available and enabled for processors to support KVM. Although originally designed for the x86 hardware, it has also been ported to FreeBSD, S/390, PowerPC, IA-64, and ARM as well.

![A High-Level Overview of the KVM/QEMU Virtualization Environment](./img/img_0.png)

## Introduction to VirtualBox

[VirtualBox](https://www.virtualbox.org) is an x86 and AMD64/Intel64 virtualization product from Oracle, running on Windows, Linux, Mac OS X, and Solaris hosts and supporting guest OSes from Windows, Linux families, and others, such as Solaris, FreeBSD, and DOS. However officially, only a select few of the most common guest OSes are supported and optimized to run on VirtualBox.

## Introduction to Vagrant

Configuring and sharing one VM is easy, but, when we have to manage multiple VMs for the same project, performing all the build and configuration steps manually can be tiresome. [Vagrant](https://www.vagrantup.com) by HashiCorp helps us automate VMs management by providing an end-to-end lifecycle management utility - the vagrant command line tool. Vagrant is a cross-platform tool, as it can be installed on Linux, Mac OS X, and Windows.
