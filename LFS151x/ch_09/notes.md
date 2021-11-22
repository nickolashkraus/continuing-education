# Chapter 9: Software-Defined Networking and Networking for Containers

## Tasks
- [x] None

## Introduction

Traditional networks cannot cope with the kind of demand mobile devices, cloud computing, and other similar technologies are generating. CPU and memory become more and more decentralized. Compute units such as VMs, services, microservices, and containers follow global deployment and availability patterns, which introduce new challenges in the management of such distributed architectures. With the advancement in compute and storage virtualization, we can quickly meet distributed compute and storage requirements. However, the most important piece of the puzzle, the one that aids in keeping all distributed units connected, is the network. Therefore, to connect devices, applications, VMs, and containers, we need a similar level of flexibility which can only be achieved by virtualizing the networking. This will allow us to meet the end-to-end business requirements.

**Software-Defined Networking** (**SDN**) decouples the network control layer from the traffic forwarding layer. This allows SDN to program the control layer and to create custom rules in order to meet these new networking requirements.

Part of this course has covered containers extensively. Next, let's take a look at container networking and see how it is one of the use cases for SDN.

## SDN Architecture

In networking, there are three distinctive planes:

**Data Plan**
* The Data Plane, also called the Forwarding Plane, is responsible for handling data packets and applyign actions to them based on rules which we program into lookup-tables.

**Control Plan**
* The Control Plane is tasked with calculating and programming the actions for the Data Plane. This is where the forwarding decisions are made and where services such as Quality of Service (QoS) and VLANs are implemented.

**Management Plane**
* The Management Plane is the place where we can configure, monitor, and manage the network devices.

## Activities Performed by a Network Device

Every network device has to perform three distinct activities:

**Ingress and egress packets**
* These are performed by the lowest layer, which decides what to do with ingress packets and which packets to forward based on forwarding tables. These activities are mapped as Data Plane activities. All routers, switches, modem, etc. are part of this plane.

**Collect, process, and manage the network information**
* By collecting, processing, and managing the network information, the network device makes the forwarding decisions, which the Data Plane follows. These activities are mapped by the Control Plane activities. Some of the protocols which run on the Control Plane are routing and adjacent device discovery.

**Monitor and manage the network**
* Using the tools available in the Management Plane, we can interact with the network device to configure it and monitor it with tools like SNMP (Simple Network Management Protocol).

In [Software-Defined Networking](https://en.wikipedia.org/wiki/Software-defined_networking), we decouple the Control Plane from the Data Plane. The Control Plane has a centralized view of the overall network, which allows it to create forwarding tables of interest. These tables are then submitted to the Data Plane to manage network traffic.

![The SDN Framework](./img/img_0.jpg)

The Control Plane has well-defined APIs that receive requests from applications to configure the network. After preparing the desired state of the network, the Control Plane communicates that to the Data Plane (also known as the Forwarding Plane), using a well-defined protocol like [OpenFlow](http://en.wikipedia.org/wiki/OpenFlow).

We can use configuration tools like Ansible or Chef to configure SDN, adding lots of flexibility and agility on the operations side as well.

## Introduction to Networking for Containers

Similar to VMs, we need to connect containers running on the same host and containers running on different hosts. The host uses the [network namespace](https://lwn.net/Articles/580893) feature of the Linux kernel to isolate the network from one container to another on the system. Network namespaces can be shared between containers as well.

On a single host, when using the virtual Ethernet (`vEth`) feature with Linux bridging, we can give a virtual network interface to each container and assign it an IP address. With technologies like [Macvlan](https://docs.docker.com/network/macvlan) and [IPVlan](https://www.kernel.org/doc/Documentation/networking/ipvlan.txt) we can configure each container to have a unique world-wide routable IP address.

As of now, if we want to achieve multi-host networking with containers, the most common solution is to use some form of Overlay network driver, which encapsulates the Layer 2 traffic to a higher layer. Examples of this type of implementation are the Docker Overlay Driver, Flannel, and Weave. Other types of implementations are also available, such as Project Calico, which allows multi-host networking on Layer 3 using BGP (Border Gateway Protocol).

## Container Networking Standards

Two different standards have been proposed so far for container networking:

[**Container Network Model**](https://github.com/docker/libnetwork/blob/master/docs/design.md) (**CNM**)

Docker, Inc. is the primary driver for this networking model. It is implemented using the [libnetwork](https://github.com/moby/libnetwork#libnetwork---networking-for-containers) project, which has the following utilizations:
1. **Null**: NOOP implementation of the driver. It is used when no networking is required.
2. **Bridge**: It provides a Linux-specific bridging implementation based in Linux Bridge.
3. **Overlay**: It provides a [multi-host communication](https://github.com/docker/libnetwork/blob/master/docs/overlay.md) over VXLAN.
4. **Remote**: It does not provide a driver. Instead, it provides a means of supporting drivers over a remote transport, for which we can write third-party drivers.

[**Container Networking Interface**](https://github.com/containernetworking/cni) (**CNI**)

Container Networking Interface is a Cloud Native Computing Foundation (CNCF) project which consists of specifications and libraries for writing plugins to configure network interfaces in Linux containers, along with a number of supported plugins. It is limited to providing network connectivity of containers and removing allocated resources when the container is deleted. As such, it has a wide range of support. It is used by projects like Kubernetes, OpenShift, and Cloud Foundry.

## Service Discovery

Now that we provided an overview of networking, let's take a moment to discuss **service discovery** as well. This becomes extremely important when we are looking to implement multi-host networking and use some form of container orchestration with Docker Swarm or Kubernetes.

Service discovery is a mechanism by which processes and services can find each other automatically and talk to each other. With respect to containers, it is used to map a container name with its IP address, so that we can access the container directly by its name without worrying about its exact location (IP address) which may change during the life of the container.

Service discovery is achieved in two steps:

**Registration**
* When a container starts, the container scheduler registers the container name to the container IP mapping in a key-value store such as **etcd** or **Consul**. And, if the container restarts or stops the scheduler updates the mapping accordingly.

**Lookup**
* Services and applications use *Lookup* to retrieve the IP address of a container so that they can connect to it. Generally, this is supported by DNS (Domain Name Server), which is local to the environment. The DNS used resolves the requests by looking at the entries in the key-value store, which is used for *Registration*. SkyDNS and Mesos-DNS are examples of such DNS services.

## Kubernetes Networking

As we know, the smallest deployment unit in Kubernetes is a Pod, which may include one or more containers. Kubernetes assigns a unique IP address to each Pod. Containers in a Pod share the same network namespace and can refer to each other by localhost. We have seen an example of network namespaces sharing between Docker containers earlier, a prior section. Containers in a Pod can expose unique ports, and become accessible through the same Pod IP address.

As each Pod gets a unique IP, Kubernetes assumes that Pods should be able to communicate with each other, irrespective of the nodes they get scheduled on. There are different ways to achieve this. Kubernetes introduced the [Container Network Interface](https://github.com/containernetworking/cni/blob/master/SPEC.md) (CNI) specification for container networking together with the following requirements that need to be implemented by Kubernetes networking driver developers:

* All pods on a node can communicate with all pods on all nodes without NAT
* All nodes can communicate with all pods (and vice versa) without NAT
* The IP that a pod sees itself as is the same IP that other pods see it as
