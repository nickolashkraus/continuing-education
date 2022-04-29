# Chapter 6: Containers: Container Orchestration

## Tasks
- [ ] Read [The Raft Consensus Algorithm](https://raft.github.io)

## Introduction

Running containers on a single node may no longer satisfy the needs of an enterprise, where, especially in production, containerized workload needs to be managed at scale. That is how we benefit the most from containers. To run containers in a multi-host environment at scale, we need to find solutions to numerous issues, summarized below:

* How to group multiple hosts together to form a cluster, and manage them as a single compute unit?
* How to schedule containers to run on specific hosts?
* How can containers running on one host communicate with containers running on other hosts?
* How to provide the container with dependent storage, when it is scheduled on a specific host?
* How to access containers over a service name, instead of accessing them directly through their IP addresses?

**Container orchestration** tools, together with different plugins (for networking and storage), aim to address the issues mentioned above.

Container orchestration is an umbrella term that encompasses container scheduling and cluster management. Container scheduling allows us to decide on which host a container or a group of containers should be deployed. With cluster management orchestrators, we can manage the resources of cluster nodes, as well as add or delete nodes from the cluster. Some of the available solutions for container orchestration are:

* Docker Swarm
* Kubernetes
* Mesos Marathon
* Nomad
* Amazon ECS

## Introduction to Docker Swarm

[Docker Swarm](https://docs.docker.com/engine/swarm) is a native container orchestration solution from [Docker, Inc](https://www.docker.com). Docker, in swarm mode, logically groups multiple Docker Engines into a swarm, or cluster, that allows for applications to be deployed and managed at scale.

![Swarm Cluster Components](./img/img_0.png)

The above illustration depicts two major components of a swarm:

**Swarm Manager Nodes**
* Accept commands on behalf of the cluster and make scheduling decisions. They also maintain the cluster state and store it using the *Internal Distributed State Store*, which uses the [Raft](https://raft.github.io) consensus algorithm. One or more nodes can be configured as managers for fault-tolerance. When multiple managers are present they are configured in *active/passive* modes.

**Swarm Worker Nodes**
* Run the Docker Engine and the sole purpose of the worker nodes is to run the container workload dispatched by the manager node(s).

## Introduction to Kubernetes

[Kubernetes](https://kubernetes.io) is an Apache 2.0-licensed open source project for automating deployment, operations, and scaling of containerized applications. It was started by Google in 2014, but many other companies like Docker, Red Hat, and VMware contributed to its success.

In July of 2015, [Cloud Native Computing Foundation](https://cncf.io) (CNCF), the nonprofit organization dedicated to advancing the development of cloud-native applications and services and driving alignment among container technologies, accepted Kubernetes as its first hosted project. The IP was transferred to CNCF from Google.

Kubernetes supports several container runtimes such as Docker, CRI-O, frakti, and rkt to run containers.

## The Kubernetes Architecture

The Kubernetes architecture and its key components are illustrated in the diagram below:

![Kubernetes Architecture](./img/img_1.png)

## The Kubernetes Architecture - Key Components (Part I)

The key components of the Kubernetes architecture are:

**Cluster**
* The cluster is a group of systems (bare-metal or virtual) and other infrastructure resources used by Kubernetes to run containerized applications.

**Master Node**
* The master is a system that takes pod scheduling decisions and manages the worker nodes. Its main components are **kube-apiserver**, **etcd**, **kube-scheduler**, and **kube-controller-manager** and they coordinate tasks around container workload scheduling, deployment, scaling, self-healing, state persistence, and delegation of other tasks to worker node agents. Multiple master nodes may be found in clusters as a solution for High Availability.

**Worker Node**
* A system on which containers are scheduled to run in pods. The node runs a daemon called **kubelet** to communicate with the master node by reporting node status information and receiving instructions from the master about container lifecycle management (this daemon is also found on master nodes). **kube-proxy**, a network proxy running on all nodes, allows applications running in the cluster to be accessed from the external world.

**Namespace**
* The namespace allows us to logically partition the cluster into virtual sub-clusters, for projects, applications, users, and teams' isolation.

## The Kubernetes Architecture - Key Components (Part II)

The key API resources of the Kubernetes architecture:

**Pod**
* The pod is a co-located group of containers with shared volumes, however, it often manages a single container. It is the smallest deployment unit in Kubernetes. A pod can be created independently, but it is recommended the use of controllers such as the ReplicaSet or Deployment, even if only a single pod is being deployed.

**ReplicaSet**
* The ReplicaSet is a mid-level controller that manages the lifecycle of pods. It ensures that the desired number of pods is running at all times.

**Deployment**
* The Deployment is a top-level controller that allows us to provide declarative updates for pods and ReplicaSets. We can define Deployments to create new resources, or replace existing ones with new ones. Some typical use cases are presented below:
1. Create a Deployment to bring up a ReplicaSet and pods.
2. Check the status of a Deployment to see if it succeeds or not.
3. Later, update that Deployment to recreate the pods (to use a new image).
4. Rollback to an earlier Deployment revision if the current Deployment isn't stable.
5. Pause and resume a Deployment. Below we provide a [sample deployment](http://kubernetes.io/docs/user-guide/deployments):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
    template:
      metadata:
        labels:
          app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.17.9
        ports:
        - containerPort: 80
```

**Service**
* The service groups sets of pods together and provides a way to refer to them from a single static IP address and the corresponding DNS name. It can reference a single pod, a group of individual pods, or pods managed by ReplicaSets or Deployments. Below, we provide a [sample service](https://kubernetes.io/docs/concepts/services-networking/service):

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
  labels:
    app: dockchat
    tier: frontend
spec:
  type: LoadBalancer
  ports:
  - port: 9500
    targetPort: 5000
  selector:
    app: dockchat
    tier: frontend
```

**Label**
* The label is an arbitrary key-value pair that is attached to a resource like a pod or a ReplicaSet. In the example above, we defined labels as **app** and **tier**.

**Selector**
* Selectors enable us to group resources based on labels. In the above example, the **frontend** service will select all pods which have the labels **app==dockchat** and **tier==frontend**.

**Volume**
* The volume is an external filesystem or storage which is available to pods and is mounted on a container's filesystem. They are built on top of [Docker volumes](https://docs.docker.com/userguide/dockervolumes).

## Introduction to Apache Mesos

When we install and set up a physical machine, we generally use it for very specific purposes, such as running applications or frameworks as Hadoop, Spark, containers, or Jenkins. Some applications might not be using all the system resources (e.g. CPU, memory) while others might be starving for more. Therefore, it would be helpful to have the ability to combine all the physical resources across multiple machines and execute tasks on specific machines based on resource requirements.

[Apache Mesos](http://mesos.apache.org) was created with this idea in mind so that we can optimally use the resources available, even if we are running disparate applications on a pool of nodes. Mesos is a distributed systems kernel that runs on every machine and introduces a level of abstraction that exposes APIs for resource management and workload scheduling. It helps us treat a cluster of nodes as a single compute unit, which manages CPU, memory, and other cluster resources provisioned across datacenter and cloud environments.

Mesos provides functionality that crosses between Infrastructure as a Service (IaaS) and Platform as a Service (PaaS). It is an open source Apache project.

## Introduction to Nomad

[HashiCorp Nomad](https://www.nomadproject.io) is a cluster manager and resource scheduler from [HashiCorp](https://www.hashicorp.com), which is distributed, highly available, and scales to thousands of nodes. It is designed to run microservices and batch jobs, and it supports different types of workloads, from containers (Docker) and VMs, to individual applications. In addition, it is capable of scheduling applications and services on different platforms like Linux, Windows, and Mac, both on-premise and clouds.

It is distributed as a single binary, which has all of its dependency and runs in a *server* and *client* mode. To submit a job, the user has to define it using a declarative language called [HashiCorp Configuration Language](https://github.com/hashicorp/hcl) (HCL) with its resource requirements. Once submitted, Nomad will find available resources in the cluster and run it to maximize resource utilization.

Below we provide a sample job file:

```hcl
# Define the hashicorp/web/frontend job
job "hashicorp/web/frontend" {

    # Job should run in the "us" region
    region = "us"

    # Run in two datacenters
    datacenters = ["us-west-1", "us-east-1"]

   # Only run workload on linux
    constraint {
        attribute = "$attr.kernel.name"
        value = "linux"
    }

    # Configure the job for rolling updates
    update {
        # Stagger updates every 30 seconds
        stagger = "30s"

        # Update a single task at a time
        max_parallel = 1
    }

    # Define the task group together with an individual task (unit of work)
    group "frontend" {
        # Ensure we have enough servers to handle traffic
        count = 10

        task "web" {
            # Use Docker to run our server
            driver = "docker"
            config {
                image = "hashicorp/web-frontend:latest"
            }

            # Specify resource limits
            resources {
                cpu = 500
                memory = 128
                network {
                    mbits = 10
                    dynamic_ports = ["http"]
                }
            }
        }
    }
}
```

which would use 10 containers from the **hashicorp/web-frontend:latest** Docker image.

## Introduction to Amazon ECS

[Amazon Elastic Container Service](https://aws.amazon.com/ecs) is part of the Amazon Web Services (AWS) offerings. It provides a fast, secure, and highly scalable container management service that makes it easy to run, stop and manage Docker containers on a cluster.

It can be configured in the following two launch modes:

**Fargate Launch Type**
* AWS Fargate allows us to run containers without managing servers and clusters. In this mode, we just have to package our applications in containers along with CPU, memory, networking, and IAM policies. We don't have to provision, configure, and scale clusters of virtual machines to run containers, as AWS will take care of it for us.

![Fargate Launch Type](./img/img_2.png)

**EC2 Launch Type**
* With the EC2 launch type, we can provision, patch, and scale the ECS cluster. This gives more control to our servers and provides a range of customization options.

![EC2 Launch Type](./img/img_3.png)
