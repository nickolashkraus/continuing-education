# Chapter 2: Infrastructure as a Service (IaaS)

## Tasks
- [x] None

## Introduction

**Infrastructure as a Service (IaaS)** is a cloud service model that provides on-demand physical and virtual computing resources, storage, network, firewall, and load balancers. To provide virtual computing resources, IaaS uses hypervisors, such as Xen, KVM, VMware ESXi, Hyper-V, or Nitro.

## Introduction to Amazon EC2

Amazon EC2 instances are in fact Virtual Machines. When provisioning EC2 instances on the AWS infrastructure, we are provisioning VMs on top of hypervisors that run directly on Amazon's physical infrastructure.

At the heart of Amazon EC2 service, are various type-1 hypervisors, such as Xen, KVM, and [Nitro](https://aws.amazon.com/ec2/nitro) (a newer KVM-based lightweight hypervisor that delivers close to bare-metal performance).

## Introduction to Azure Virtual Machine

The [Azure Virtual Machine](https://docs.microsoft.com/en-us/azure/virtual-machines) service allows individual users and enterprises alike to provision and manage compute resources, both from a web Portal or the [Azure Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview), a flexible command line utility configurable to use either Bash or PowerShell.

Azure cloud services are enabled by the Azure Hypervisor, a customized version of Microsoft Hyper-V type-1 hypervisor.

## Introduction to DigitalOcean Droplet

DigitalOcean helps you create a simple cloud quickly, as it promises IaaS virtual instances launched in as little as 55 seconds.

In the DigitalOcean ecosystem, the virtual compute instances are [Droplets](https://www.digitalocean.com/products/droplets), and they are launched on top of the KVM type-1 hypervisor, with SSD (Solid-State Drive) as their primary storage disk.

## Introduction to Google Compute Engine

[Google Compute Engine](https://cloud.google.com/compute) (GCE) service allows individual users and enterprises alike to build a reliable, flexible, and secure cloud infrastructure for their applications and workloads on Google's platform.

GCE services are enabled by the KVM type-1 hypervisor, running directly on Google's physical infrastructure and allowing for VMs to be launched with Linux and Windows guest Operating Systems.

## Introduction to IBM Cloud Virtual Servers

[IBM Cloud](https://www.ibm.com/cloud) is one of the leading cloud services providers for enterprises and academic institutions. It includes the typical service models, the Infrastructure as a Service (IaaS), Software as a Service (SaaS), and Platform as a Service (PaaS) offered through public, private, and hybrid cloud delivery models. Part of the IBM Cloud IaaS model is the [IBM Cloud Virtual Servers](https://www.ibm.com/cloud/virtual-servers) service, also known as Virtual Machines, one of the many services providing cloud Compute services. IBM Cloud Virtual Servers provide interoperability between virtual and bare-metal servers by being deployed on the same VLANs as the physical servers.

IBM Cloud is the successor of two joined technologies - the IBM mainframe and virtualization. IBM treats virtualization with some level of flexibility. While it uses IBM z/VM and IBM PowerVM, two powerful hypervisors, to manage its own virtual workload, the users of IBM Cloud are allowed to choose between XenServer, VMware, and Hyper-V hypervisors when managing bare-metal instances. This level of hypervisor flexibility is one of the advantages of IBM Cloud, and one of the top reasons why users pick IBM Cloud over another cloud service provider.

## Introduction to Oracle Cloud Compute Virtual Machines

Oracle is a leading cloud services provider through its [Oracle Cloud](https://www.oracle.com/cloud) offering. A commonly used infrastructure product is [Cloud Compute](https://www.oracle.com/cloud/compute), which includes several options to support various types of I/O intensive workloads, high-performance computing (HPC), and artificial intelligence (AI). These options are represented by Cloud Compute Instances that include Virtual Machines (VMs), Bare Metal Servers, GPU optimized VMs and Bare Metal Servers, alongside Autonomous Linux instances, and instances optimized for Container Registries and Container Engines for Kubernetes.

At the core of Oracle Cloud Infrastructure are Bare Metal Servers capable of supporting Nested Virtualization when paired with a hypervisor such as KVM.

## Introduction to OpenStack

With [OpenStack](https://www.openstack.org), we can build a cloud computing platform for public and private clouds. OpenStack was started as a joint project between Rackspace and NASA in 2010. In 2012, a non-profit corporate entity, the OpenStack Foundation, was formed and it is managing it since then while it receives the support of more than 500 organizations. OpenStack is an open source software platform released under an Apache 2.0 License.
