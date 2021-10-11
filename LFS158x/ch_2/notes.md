# Chapter 2: Container Orchestration

## Tasks
- [x] None

## Introduction

With container images, we confine the application code, its runtime, and all of its dependencies in a pre-defined format. And, with container runtimes like **runC**, **containerd**, or **cri-o** we can use those pre-packaged images, to create one or more containers. All of these runtimes are good at running containers on a single host. But, in practice, we would like to have a fault-tolerant and scalable solution, which can be achieved by creating a single **controller/management** unit, after connecting multiple nodes together. This controller/management unit is generally referred to as a **container orchestrator**.

## What Are Containers?

**Containers** are an application-centric method to deliver high-performing, scalable applications on any infrastructure of your choice. Containers are best suited to deliver microservices by providing portable, isolated virtual environments for applications to run without interference from other running applications.

![Container Deployment](./img/img_0.png)

**Microservices** are lightweight applications written in various modern programming languages, with specific dependencies, libraries and environmental requirements. To ensure that an application has everything it needs to run successfully it is packaged together with its dependencies.

Containers encapsulate microservices and their dependencies but do not run them directly. Containers run container images.

A **container image** bundles the application along with its runtime, libraries, and dependencies, and it represents the source of a container deployed to offer an isolated executable environment for the application. Containers can be deployed from a specific image on many platforms, such as workstations, Virtual Machines, public cloud, etc.

## What Is Container Orchestration?

**Container orchestrators** are tools which group systems together to form clusters where containers' deployment and management is automated at scale while meeting the requirements mentioned above.

## Container Orchestrators

Although not exhaustive, the list below provides a few different container orchestration tools and services available today:

* **Amazon Elastic Container Service**
[Amazon Elastic Container Service (ECS)](https://aws.amazon.com/ecs) is a hosted service provided by Amazon Web Services (AWS) to run Docker containers at scale on its infrastructure.

* **Azure Container Instances**
[Azure Container Instance (ACI)](https://azure.microsoft.com/en-us/services/container-instances) is a basic container orchestration service provided by Microsoft Azure.

* **Azure Service Fabric**
[Azure Service Fabric](https://azure.microsoft.com/en-us/services/service-fabric) is an open source container orchestrator provided by Microsoft Azure.

* **Kubernetes**
[Kubernetes](https://kubernetes.io) is an open source orchestration tool, originally started by Google, today part of the Cloud Native Computing Foundation (CNCF) project.

* **Marathon**
[Marathon](https://mesosphere.github.io/marathon) is a framework to run containers at scale on Apache Mesos.

* **Nomad**
[Nomad](https://www.nomadproject.io) is the container and workload orchestrator provided by HashiCorp.

* **Docker Swarm**
[Docker Swarm](https://docs.docker.com/engine/swarm) is a container orchestrator provided by Docker, Inc. It is part of Docker Engine.
