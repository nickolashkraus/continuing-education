# Chapter 19: Serverless Computing

## Tasks
- [x] None

## Introduction

**Serverless computing** or just **serverless** is a method of running applications without concerns about the provisioning of computer servers or any of the compute resources. However, behind the scenes, compute servers are most definitely involved. Serverless is similar to the wireless Internet where the wires exist, but they are not visible to end-users.

Serverless computing requires compute resources to run applications, but the server management and capacity planning decisions are completely abstracted from developers and users.

## Serverless Computing

In serverless computing, we generally write applications/functions that focus and master one particular task. We then upload that application on the cloud provider, which gets invoked via different events, such as HTTP requests, webhooks, etc.

The most common use case of serverless computing is to run any stateless applications like data processing or real-time stream processing. However, it can also augment a stateful application. Internet of Things and ChatBots are common use cases of serverless computing.

All major cloud providers like AWS, Google Cloud Platform, or Microsoft Azure include serverless offerings. We will explore them in this chapter. We can also build our own serverless solutions on container orchestrators like Docker Swarm, and Kubernetes.

Most often, serverless computing is referred via services offered by cloud providers. We will also follow the same practice. In addition, we will explicitly talk about custom or self-managed serverless computing whenever needed.

## Features and Benefits of Serverless Computing

Key features and benefits of serverless computing include:

**No Server Management**
* When we use serverless offerings by cloud providers, no server management and capacity planning is required by us. It is all handled by the cloud services provider.

**Cost-Effective**
* We only need to pay for a CPU time when our applications/functions are executed. There is no charge when code is not running. Also, there is no need to rent or purchase fixed quantities of servers.

**Flexible Scaling**
* We don’t need to set up or tune autoscaling. Applications are automatically scaled up/down based on demand.

**Automated High Availability and Fault Tolerance**
* High availability and fault tolerance are automatically included by the underlying cloud infrastructure providers. Developers are not required to specifically program for such features.

## Drawbacks of Serverless Computing

There are some drawbacks of serverless computing that include:

**Vendor Lock-In**
* Serverless features and implementations vary from vendor to vendor. So, the same application/function may not behave the same way if you change the provider, and changing your provider can incur additional expenses.

**Multitenancy and Security**
* You cannot be sure what other applications/functions run alongside yours, which raises multitenancy and security concerns.

**Performance**
* If the application is not in use, the service provider can take it down, which will affect performance.

**Resource Limits**
* Cloud providers set resource limits for our serverless applications/functions. Therefore, it is safer not to run high-performance, or resource-intensive workloads using serverless solutions.

**Monitoring and Debugging**
* It is more challenging to monitor serverless applications compared to applications running on traditional servers.
