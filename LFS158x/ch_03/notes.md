# Chapter 3: Kubernetes

## Tasks
- [x] None

## What Is Kubernetes?

According to the [Kubernetes](https://kubernetes.io) website,

>"Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications."

## From Borg to Kubernetes

According to the abstract of Google's [Borg paper](https://research.google.com/pubs/pub43438.html), published in 2015,

>"Google's Borg system is a cluster manager that runs hundreds of thousands of jobs, from many thousands of different applications, across a number of clusters each with up to tens of thousands of machines."

Some of the features/objects of Kubernetes that can be traced back to Borg, or to lessons learned from it, are:

* API servers
* Pods
* IP-per-Pod
* Services
* Labels

## Kubernetes Features I

Kubernetes offers a very rich set of features for container orchestration. Some of its fully supported features are:

* **Automatic bin packing**
Kubernetes automatically schedules containers based on resource needs and constraints, to maximize utilization without sacrificing availability.

* **Self-healing**
Kubernetes automatically replaces and reschedules containers from failed nodes. It kills and restarts containers unresponsive to health checks, based on existing rules/policy. It also prevents traffic from being routed to unresponsive containers.

* **Horizontal scaling**
With Kubernetes applications are scaled manually or automatically based on CPU or custom metrics utilization.

* **Service discovery and Load balancing**
Containers receive their own IP addresses from Kubernetes, while it assigns a single Domain Name System (DNS) name to a set of containers to aid in load-balancing requests across the containers of the set.

## Kubernetes Features II

Some other fully supported Kubernetes features are:

* **Automated rollouts and rollbacks**
Kubernetes seamlessly rolls out and rolls back application updates and configuration changes, constantly monitoring the application's health to prevent any downtime.

* **Secret and configuration management**
Kubernetes manages sensitive data and configuration details for an application separately from the container image, in order to avoid a re-build of the respective image. Secrets consist of sensitive/confidential information passed to the application without revealing the sensitive content to the stack configuration, like on GitHub.

* **Storage orchestration**
Kubernetes automatically mounts software-defined storage (SDS) solutions to containers from local storage, external cloud providers, distributed storage, or network storage systems.

* **Batch execution**
Kubernetes supports batch execution, long-running jobs, and replaces failed containers.

There are many additional features currently in alpha or beta phase. They will add great value to any Kubernetes deployment once they become stable features. For example, support for role-based access control (RBAC) is stable only as of the Kubernetes 1.8 release.

## Cloud Native Computing Foundation (CNCF)

The [Cloud Native Computing Foundation](https://www.cncf.io) (CNCF) is one of the projects hosted by [the Linux Foundation](https://www.linuxfoundation.org). CNCF aims to accelerate the adoption of containers, microservices, and cloud-native applications.

Graduated projects:
* [Kubernetes](https://kubernetes.io) for container orchestration
* [Prometheus](https://prometheus.io) for monitoring
* [Envoy](https://github.com/envoyproxy/envoy) for service mesh
* [CoreDNS](https://coredns.io) for service discovery
* [containerd](http://containerd.io) for container runtime
* [Fluentd](http://www.fluentd.org) for logging
* [Harbor](https://goharbor.io) for registry
* [Helm](https://www.helm.sh) for package management
* [Vitess](http://vitess.io) for cloud-native storage
* [Jaeger](https://github.com/jaegertracing/jaeger) for distributed tracing
* [TUF](https://github.com/theupdateframework/specification) for software updates
* [TiKV](https://tikv.org) for key/value store

Inbating projects:
* [CRI-O](https://cri-o.io) for container runtime
* [Linkerd](https://linkerd.io) for service mesh
* [Contour](https://projectcontour.io) for ingress
* [etcd](https://github.com/etcd-io) for key/value store
* [gRPC](http://www.grpc.io) for remote procedure call (RPC)
* [CNI](https://github.com/containernetworking/cni) for networking API
* [Rook](https://github.com/rook/rook) for cloud-native storage
* [Notary](https://github.com/theupdateframework/notary) for security
* [NATS](https://nats.io) for messaging
* [OpenTracing](http://opentracing.io) for distributed tracing
* [Open Policy Agent](https://www.openpolicyagent.org) for policy
* And many [more](https://www.cncf.io/projects)
