# Chapter 17: Service Mesh

## Tasks
- [x] None

## Introduction

**Service mesh** is a network communication infrastructure layer for a microservices-based application. When multiple microservices are communicating with each other, a service mesh allows us to decouple resilient communication patterns such as circuit breakers and timeouts from the application code.

## Features and Implementation of Service Mesh

Service mesh is generally implemented using a sidecar proxy. A sidecar is a container that runs alongside the primary application and complements it with additional features like logging, monitoring, and traffic routing. In the service mesh architecture, the sidecar pattern implements inter-service communication, monitoring, or other features that can be decoupled and abstracted away from individual services.

The following are some of the features of service mesh:

**Communication**
* It provides flexible, reliable, and fast communication between various service instances.

**Circuit Breakers**
* It restricts traffic to unhealthy service instances.

**Routing**
* It passes a REST request for /foo from the local service instance, to which the service is connected.

**Retries and Timeouts**
* It can automatically retry requests on certain failures and can timeout requests after a specified period.

**Service Discovery**
* It discovers healthy, available instances of services.

**Observability**
* It monitors latency, traces traffic flow, and generates access logs.

**Authentication and Authorization**
* It can authenticate and authorize incoming requests.

**Transport Layer Security (TLS) Encryption**
* It can secure service-to-service communication using TLS.

## Data Plane and Control Plane

Similar to the Software-Defined Networking explored earlier, a service mesh also features Data and Control Planes.

**Service Mesh Data Plane**
* It provides features that we mentioned on the previous page. It touches every packet/request in the system.

**Service Mesh Control Plane**
* It provides policy and configuration for the Data Plane. For example, by using the control plane, we can specify settings for load balancing and circuit breakers.

## Service Mesh Project Landscape

There are many service mesh projects, which can be divided into two categories:

**Data Plane**
* Linkerd
* NGINX
* HAProxy
* Envoy
* Traefik/Maesh

**Control Plane**
* Istio
* Nelson
* SmartStack

## Introduction to Consul

[Consul](https://www.consul.io) by [HashiCorp](https://www.hashicorp.com) is an open source project aiming to provide a secure multi-cloud service networking through automated network configuration and service discovery.

Consul’s strength lies in its support to connect services running in multiple datacenters. It is highly available for fault tolerance and increased performance, while it supports thousands or tens of thousands of simultaneous client services.

Client services are capable of automatic discovery of servers, while a distributed agent and node failure detection supports scalability more than the traditional heartbeating schemes. Very low coupling between datacenters, together with failure detection, connection caching, and multiplexing ensures fast and reliable cross-datacenter requests.

Consul forwards RPC requests between remote Consul servers when a request is made for a resource available in another datacenter. If the remote datacenter is not available, then the remote resources will not be available either.

Data caching is also supported by Consul, like connect certificates or optional results, which help with local decisions making about incoming connection requests even if the connection between servers is disrupted or if the servers are temporarily unavailable.

## Introduction to Envoy

[Envoy](https://www.envoyproxy.io) is a [Cloud Native Computing Foundation](https://www.cncf.io) (CNCF) project, which was originally built by [Lyft](https://www.lyft.com). It is an open source project that provides an L7 proxy and communication bus for large, modern, service-oriented architectures.

Envoy has an out-of-process architecture, which means it is not dependent on the application code. It runs alongside the application and communicates with the application on localhost. We referred to this earlier as a sidecar pattern.

With the Envoy sidecar implementation, applications need not be aware of the network topology. Envoy can work with any language and can be managed independently.

Envoy can be configured as *service* and *edge* proxy. In the service type of configuration, it is used as a communication bus for all traffic between microservices. With the edge type of configuration, it provides a single point of ingress to the external world.

## Introduction to Istio

[Istio](https://istio.io) is one of the most popular service mesh solutions. It is an open source platform, backed by companies like Google, IBM and Lyft.

Istio is divided into the following two planes:

**Data Plane**
* It is composed of a set of Envoy proxies deployed as sidecars to provide a medium for communication and to control all network communication between microservices.

**Control Plane**
* It manages and configures proxies to route traffic, enforces policies at runtime, and collects telemetry. The control plane includes the Citadel, Gallery, and Pilot.

![Istio Architecture](./img/img_0.svg)

## Istio Architecture

The main components of Istio are:

**Envoy Proxy**
* Istio uses an extended version of the Envoy proxy, using which it implements features like dynamic service discovery, load balancing, TLS termination, circuit breakers, health checks, etc. Envoy is deployed as sidecars.

**Istiod**
* Istiod provides service discovery, configuration, and certificate management.

## Introduction to Kuma

[Kuma](https://kuma.io) by [Kong](https://konghq.com) is a platform-agnostic, modern and universal open source control plane for Service Mesh. It can be easily setup on VMs, bare-metal, and on Kubernetes.

Kuma is based on the Envoy proxy - a sidecar proxy designed for cloud-native applications, that supports monitoring, security, and reliability for microservice applications at scale. With Envoy used as a data plane, Kuma can handle any L4/L7 traffic to observe and route traffic between services.

While Kuma can be used by novices, it also provides data plane configuration policies for more experienced Service Mesh users.

## Introduction to Linkerd

[Linkerd](https://linkerd.io) is an open source network proxy and one of the [Cloud Native Computing Foundation](https://courses.edx.org/xblock/cncf.io) (CNCF) projects.

It supports all features of the service meshes listed earlier. In addition, Linkerd can be installed per host or instance, as a replacement for the sidecar deployment.

## Introduction to Maesh

[Maesh](https://containo.us/maesh) is a simple open source service mesh, from [Containous](https://containo.us), the developers of [Traefik](https://containo.us/traefik) ingress proxy. It is a simple and easy to configure service mesh, that provides traffic visibility and management inside a Kubernetes cluster.

Maesh improves cluster security through monitoring, logging, visibility, and access controls. Communication monitoring and tracing also help with traffic optimization and increased application performance.

Its simplicity and ease of implementation allow administrators to spend their valuable time focusing on business applications.

Maesh is able to reveal underutilized resources, or overloaded services, which helps with proper resources allocation.

## Introduction to Tanzu Service Mesh

[Tanzu Service Mesh](https://tanzu.vmware.com/service-mesh) (briefly introduced as [NSX Service Mesh](https://blogs.vmware.com/networkvirtualization/2019/11/nsx-service-mesh-on-vmware-tanzu.html)) is an enterprise-class service mesh developed by [VMware](https://vmware.com), built on top of [VMware NSX](https://www.vmware.com/products/nsx.html). It aims to simplify the connectivity, security, and monitoring of applications on any runtime and on any cloud. In addition, as a modern distributed solution, it brings together application owners, DevOps, SRE, and SecOps.
