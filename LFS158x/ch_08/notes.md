# Chapter 8: Kubernetes Building Blocks

## Tasks
- [x] None

## Introduction

In this chapter, we will explore the **Kubernetes object model**and describe some of its fundamental building blocks, such as **Pods**, **ReplicaSets**, **Deployments**,**Namespaces**, etc. We will also discuss the essential role of **Labels**and **Selectors** in a microservices driven architecture as they logically group decoupled objects together.

## Kubernetes Object Model

Kubernetes has a very rich object model, representing different persistent entities in the Kubernetes cluster. Those entities describe:
* What containerized applications we are running
* The nodes where the containerized applications are deployed
* Application resource consumption
* Policies attached to applications, like restart/upgrade policies, fault tolerance, etc.

With each object, we declare our intent, or the desired state of the object, in the `spec` section. The Kubernetes system manages the `status` section for objects, where it records the actual state of the object. At any given point in time, the Kubernetes Control Plane tries to match the object's actual state to the object's desired state.

Examples of Kubernetes objects are Pods, ReplicaSets, Deployments, Namespaces, etc. We will explore them next.

When creating an object, the object's configuration data section from below the `spec` field has to be submitted to the Kubernetes API server. The API request to create an object must have the `spec` section, describing the desired state, as well as other details. Although the API server accepts object definition files in a JSON format, most often we provide such files in a YAML format which is converted by `kubectl` in a JSON payload and sent to the API server.

Below is an example of a [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment) object's configuration manifest in YAML format:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.15.11
        ports:
        - containerPort: 80
```

The `apiVersion` field is the first required field, and it specifies the API endpoint on the API server which we want to connect to; it must match an existing version for the object type defined. The second required field is `kind`, specifying the object type - in our case it is `Deployment`, but it can be `Pod`, `ReplicaSet`, `Namespace`, `Service`, etc. The third required field `metadata`, holds the object's basic information, such as name, labels, namespace, etc. Our example shows two `spec` fields (`spec` and `spec.template.spec`). The fourth required field `spec` marks the beginning of the block defining the desired state of the Deployment object. In our example, we are requesting that 3 replicas, or 3 instances of the Pod, are running at any given time. The Pods are created using the Pod Template defined in `spec.template`. A nested object, such as the Pod being part of a Deployment, retains its `metadata` and `spec` and loses the `apiVersion` and `kind` - both being replaced by `template`. In `spec.template.spec`, we define the desired state of the Pod. Our Pod creates a single container running the `nginx:1.15.11` image from [Docker Hub](https://hub.docker.com/_/nginx).

Once the Deployment object is created, the Kubernetes system attaches the `status` field to the object and populates it with all necessary status fields.

## Pods

From the Kubernetes documentation...

> **Pods**
>
> Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
>
> A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host.
>
> As well as application containers, a Pod can contain [init containers](https://kubernetes.io/docs/concepts/workloads/pods/init-containers) that run during Pod startup. You can also inject [ephemeral containers](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers) for debugging if your cluster offers this.
>
> **What is a Pod?(())
>
> **Note**: While Kubernetes supports more container runtimes than just Docker, Docker is the most commonly known runtime, and it helps to describe Pods using some terminology from Docker.
>
> The shared context of a Pod is a set of Linux namespaces, cgroups, and potentially other facets of isolation - the same things that isolate a Docker container. Within a Pod's context, the individual applications may have further sub-isolations applied.
>
> In terms of Docker concepts, a Pod is similar to a group of Docker containers with shared namespaces and shared filesystem volumes.

[Source](https://kubernetes.io/docs/concepts/workloads/pods)

A [Pod](https://kubernetes.io/docs/concepts/workloads/pods) is the smallest and simplest Kubernetes object. It is the unit of deployment in Kubernetes, which represents a single instance of the application. A Pod is a logical collection of one or more containers, which:
* Are scheduled together on the same host with the Pod
* Share the same network namespace, meaning that they share a single IP address originally assigned to the Pod
* Have access to mount the same external storage (volumes).

![Single- and Multi-Container Pods](./img/img_0.svg)

Pods are ephemeral in nature, and they do not have the capability to self-heal themselves. That is the reason they are used with controllers which handle Pods' replication, fault tolerance, self-healing, etc. Examples of controllers are Deployments, ReplicaSets, ReplicationControllers, etc. We attach a nested Pod's specification to a controller object using the Pod Template, as we have seen in the previous section.

Below is an example of a Pod object's configuration manifest in YAML format:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.15.11
    ports:
    - containerPort: 80
```

The `apiVersion` field must specify `v1` for the `Pod` object definition. The second required field is `kind` specifying the `Pod` object type. The third required field `metadata`, holds the object's name and label. The fourth required field `spec` marks the beginning of the block defining the desired state of the Pod object - also named the `PodSpec`. Our Pod creates a single container running the `nginx:1.15.11` image from [Docker Hub](https://hub.docker.com/_/nginx). The `containerPort` field specifies the container port to be exposed by Kubernetes resources for inter-application access or external client access.

## Labels

From the Kubernetes documentation...

> *Labels* are key/value pairs that are attached to objects, such as pods. Labels are intended to be used to specify identifying attributes of objects that are meaningful and relevant to users, but do not directly imply semantics to the core system. Labels can be used to organize and to select subsets of objects. Labels can be attached to objects at creation time and subsequently added and modified at any time. Each object can have a set of key/value labels defined. Each Key must be unique for a given object.
>
> ```
> "metadata": {
>   "labels": {
>     "key1" : "value1",
>     "key2" : "value2"
>   }
> }
> ```
>
> Labels allow for efficient queries and watches and are ideal for use in UIs and CLIs. Non-identifying information should be recorded using [annotations](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations).

[Source](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels)

[Labels](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels) are **key-value pairs** attached to Kubernetes objects (e.g. Pods, ReplicaSets, Nodes, Namespaces, Persistent Volumes). Labels are used to organize and select a subset of objects, based on the requirements in place. Many objects can have the same Label(s). Labels do not provide uniqueness to objects. Controllers use Labels to logically group together decoupled objects, rather than using objects' names or IDs.

![Labels](./img/img_1.png)

In the image above, we have used two Label keys: `app` and `env`. Based on our requirements, we have given different values to our four Pods. The Label `env=dev` logically selects and groups the top two Pods, while the Label `app=frontend` logically selects and groups the left two Pods. We can select one of the four Pods - bottom left, by selecting two Labels: `app=frontend` `AND` `env=qa`.

## Label Selectors

From the Kubernetes documentation...

> Unlike [names and UIDs](https://kubernetes.io/docs/concepts/overview/working-with-objects/names), labels do not provide uniqueness. In general, we expect many objects to carry the same label(s).
>
> Via a label selector, the client/user can identify a set of objects. The label selector is the core grouping primitive in Kubernetes.
>
> The API currently supports two types of selectors: *equality-based* and *set-based*. A label selector can be made of multiple *requirements* which are comma-separated. In the case of multiple requirements, all must be satisfied so the comma separator acts as a logical AND (&&) operator.
>
> The semantics of empty or non-specified selectors are dependent on the context, and API types that use selectors should document the validity and meaning of them.
>
> **Note**: For some API types, such as ReplicaSets, the label selectors of two instances must not overlap within a namespace, or the controller can see that as conflicting instructions and fail to determine how many replicas should be present.
>
> **Caution**: For both equality-based and set-based conditions there is no logical OR (||) operator. Ensure your filter statements are structured accordingly.

[Source](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)

Controllers use [Label Selectors](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors) to select a subset of objects. Kubernetes supports two types of Selectors:

**Equality-Based Selectors**
* Equality-Based Selectors allow filtering of objects based on Label keys and values. Matching is achieved using the `=`, `==` (equals, used interchangeably), or `!=` (not equals) operators. For example, with `env==dev` or `env=dev` we are selecting the objects where the `env` Label key is set to value `dev`.

**Set-Based Selectors**
* Set-Based Selectors allow filtering of objects based on a set of values. We can use `in`, `notin` operators for Label values, and `exist/does not exist` operators for Label keys. For example, with `env in (dev,qa)` we are selecting objects where the `env` Label is set to either `dev` or `qa`; with `!app` we select objects with no Label key `app`.

![Selectors](./img/img_2.png)

## ReplicationControllers

From the Kubernetes documentation...

> Note: A [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment) that configures a [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset) is now the recommended way to set up replication.
>
> A ReplicationController ensures that a specified number of pod replicas are running at any one time. In other words, a ReplicationController makes sure that a pod or a homogeneous set of pods is always up and available.

[Source](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller)

Although no longer a recommended controller, a [ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller) ensures a specified number of replicas of a Pod is running at any given time, by constantly comparing the actual state with the desired state of the managed application. If there are more Pods than the desired count, the replication controller randomly terminates the number of Pods exceeding the desired count, and, if there are fewer Pods than the desired count, then the replication controller requests additional Pods to be created until the actual count matches the desired count. Generally, we do not deploy a Pod independently, as it would not be able to re-start itself if terminated in error because a Pod misses the much desired self-healing feature that Kubernetes otherwise promises. The recommended method is to use some type of a controller to run and manage Pods.

The default recommended controller is the [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment) which configures a [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset) controller to manage Pods' lifecycle.

## ReplicaSets

From the Kubernetes documentation...

> A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time. As such, it is often used to guarantee the availability of a specified number of identical Pods.
>
> **How a ReplicaSet works**
>
> A ReplicaSet is defined with fields, including a selector that specifies how to identify Pods it can acquire, a number of replicas indicating how many Pods it should be maintaining, and a pod template specifying the data of new Pods it should create to meet the number of replicas criteria. A ReplicaSet then fulfills its purpose by creating and deleting Pods as needed to reach the desired number. When a ReplicaSet needs to create new Pods, it uses its Pod template.
>
> A ReplicaSet is linked to its Pods via the Pods' [metadata.ownerReferences](https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/#owners-and-dependents) field, which specifies what resource the current object is owned by. All Pods acquired by a ReplicaSet have their owning ReplicaSet's identifying information within their ownerReferences field. It's through this link that the ReplicaSet knows of the state of the Pods it is maintaining and plans accordingly.
>
> A ReplicaSet identifies new Pods to acquire by using its selector. If there is a Pod that has no OwnerReference or the OwnerReference is not a Controller and it matches a ReplicaSet's selector, it will be immediately acquired by said ReplicaSet.
>
> **When to use a ReplicaSet**
>
> A ReplicaSet ensures that a specified number of pod replicas are running at any given time. However, a Deployment is a higher-level concept that manages ReplicaSets and provides declarative updates to Pods along with a lot of other useful features. Therefore, we recommend using Deployments instead of directly using ReplicaSets, unless you require custom update orchestration or don't require updates at all.
>
> This actually means that you may never need to manipulate ReplicaSet objects: use a Deployment instead, and define your application in the spec section.

[Source](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset)

A [ReplicaSet](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset) is, in part, the next-generation ReplicationController, as it implements the replication and self-healing aspects of the ReplicationController. ReplicaSets support both equality- and set-based Selectors, whereas ReplicationControllers only support equality-based Selectors.

With the help of a ReplicaSet, we can scale the number of Pods running a specific application container image. Scaling can be accomplished manually or through the use of an [autoscaler](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale).

Below we graphically represent a ReplicaSet, with the replica count set to 3 for a specific Pod template. Pod-1, Pod-2, and Pod-3 are identical, running the same application container image, being cloned from the same Pod template. For now, the current state matches the desired state.

![ReplicaSet](./img/img_3.png)

ReplicaSets can be used independently as Pod controllers but they only offer a limited set of features. A set of complementary features are provided by Deployments, the recommended controllers for the orchestration of Pods. Deployments manage the creation, deletion, and updates of Pods. A Deployment automatically creates a ReplicaSet, which then creates a Pod. There is no need to manage ReplicaSets and Pods separately, the Deployment will manage them on our behalf.

## Deployments

From the Kubernetes documentation...

> A *Deployment* provides declarative updates for Pods and ReplicaSets.
>
> You describe a *desired state* in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate. You can define Deployments to create new ReplicaSets, or to remove existing Deployments and adopt all their resources with new Deployments.
>
> **Note**: Do not manage ReplicaSets owned by a Deployment. Consider opening an issue in the main Kubernetes repository if your use case is not covered below.

[Source](https://kubernetes.io/docs/concepts/workloads/controllers/deployment)

[Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment) objects provide declarative updates to Pods and ReplicaSets. The DeploymentController is part of the master node's controller manager, and as a controller it also ensures that the current state always matches the desired state. It allows for seamless application updates and rollbacks through *rollouts* and *rollbacks*, and it directly manages its ReplicaSets for application scaling.

In the following example, a new Deployment creates ReplicaSet A which then creates 3 Pods, with each Pod Template configured to run one `nginx:1.7.9` container image. In this case, the ReplicaSet A is associated with nginx:1.7.9 representing a state of the Deployment. This particular state is recorded as Revision 1.

![Deployment](./img/img_4.png)

In time, we need to push updates to the application managed by the Deployment object. Let's change the Pods' Template and update the container image from `nginx:1.7.9` to `nginx:1.9.1`. The Deployment triggers a new ReplicaSet B for the new container image versioned `1.9.1` and this association represents a new recorded state of the Deployment, Revision 2. The seamless transition between the two ReplicaSets, from ReplicaSet A with 3 Pods versioned `1.7.9` to the new ReplicaSet B with 3 new Pods versioned `1.9.1`, or from Revision 1 to Revision 2, is a Deployment *rolling update*.

A *rolling update* is triggered when we update specific properties of the Pod Template for a deployment. While updating the container image, container port, volumes, and mounts would trigger a new Revision, other operations like scaling or labeling the deployment do not trigger a rolling update, thus do not change the Revision number.

Once the rolling update has completed, the Deployment will show both ReplicaSets A and B, where A is scaled to 0 (zero) Pods, and B is scaled to 3 Pods. This is how the Deployment records its prior state configuration settings, as Revisions.

![Deployment](./img/img_5.png)

Once ReplicaSet B and its 3 Pods versioned `1.9.1` are ready, the Deployment starts actively managing them. However, the Deployment keeps its prior configuration states saved as Revisions which play a key factor in the rollback capability of the Deployment - returning to a prior known configuration state. In our example, if the performance of the new `nginx:1.9.1` is not satisfactory, the Deployment can be rolled back to a prior Revision, in this case from Revision 2 back to Revision 1 running `nginx:1.7.9` once again.

![Deployment](./img/img_5.png)

## Namespaces

From the Kubernetes documentation...

> In Kubernetes, *namespaces* provides a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. Namespace-based scoping is applicable only for namespaced objects (e.g. Deployments, Services, etc) and not for cluster-wide objects (e.g. StorageClass, Nodes, PersistentVolumes, etc).
>
> **When to Use Multiple Namespaces**
>
> Namespaces are intended for use in environments with many users spread across multiple teams, or projects. For clusters with a few to tens of users, you should not need to create or think about namespaces at all. Start using namespaces when you need the features they provide.
>
> Namespaces provide a scope for names. Names of resources need to be unique within a namespace, but not across namespaces. Namespaces cannot be nested inside one another and each Kubernetes resource can only be in one namespace.
>
> Namespaces are a way to divide cluster resources between multiple users (via [resource quota](https://kubernetes.io/docs/concepts/policy/resource-quotas)).
>
> It is not necessary to use multiple namespaces to separate slightly different resources, such as different versions of the same software: use labels to distinguish resources within the same namespace.

[Source](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces)

If multiple users and teams use the same Kubernetes cluster we can partition the cluster into virtual sub-clusters using [Namespaces](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces). The names of the resources/objects created inside a Namespace are unique, but not across Namespaces in the cluster.

To list all the Namespaces, we can run the following command:

```bash
$ kubectl get namespaces
```

Generally, Kubernetes creates four Namespaces out of the box: **kube-system**, **kube-public**, **kube-node-lease**, and **default**. The **kube-system** Namespace contains the objects created by the Kubernetes system, mostly the control plane agents. The **default** Namespace contains the objects and resources created by administrators and developers, and objects are assigned to it by default unless another Namespace name is provided by the user. **kube-public** is a special Namespace, which is unsecured and readable by anyone, used for special purposes such as exposing public (non-sensitive) information about the cluster. The newest Namespace is **kube-node-lease** which holds node lease objects used for node heartbeat data. Good practice, however, is to create additional Namespaces, as desired, to virtualize the cluster and isolate users, developer teams, applications, or tiers.

Namespaces are one of the most desired features of Kubernetes, securing its lead against competitors, as it provides a solution to the multi-tenancy requirement of today's enterprise development teams.

[Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas) help users limit the overall resources consumed within Namespaces, while [LimitRanges](https://kubernetes.io/docs/concepts/policy/limit-range) help limit the resources consumed by Pods or Containers in a Namespace.
