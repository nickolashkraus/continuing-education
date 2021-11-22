# Chapter 7: Unikernels

## Tasks
- [x] None

## Introduction

Earlier in the course, we mentioned that in today's world our end goal is, most of the time, to run an application. We also explained how container technology is helping us achieve this end goal. We learned that, to run containers, we also need to ship the entire user-space libraries of the respective distribution with the application. In most cases, the majority of the libraries would not be consumed by the application. Therefore, it makes sense to ship the application only with the set of user-space libraries which are needed by the application.

With **unikernels**, we can also select the part of the kernel needed to run with the specific application. With unikernels, we can create a single address space executable, which has both application and kernel components. The image can be deployed on VMs or bare metal, based on the unikernel's type.

According to the [unikernels](http://unikernel.org) website,

>"Unikernels are specialised, single-address-space machine images constructed by using library operating systems."

## Creating Specialized VM Images

The Unikernel goes one step further than other technologies, creating specialized virtual machine images with just:

* The application code
* The configuration files of the application
* The user-space libraries needed by the application
* The application runtime (like JVM)
* The system libraries of the unikernel, which allow back and forth communication with the hypervisor

According to the [protection ring](https://en.wikipedia.org/wiki/Protection_ring) of the x86 architecture, we run the kernel on **ring0** and the application on **ring3**, which has the least privileges. **Ring0** has the most privileges, like access to hardware, and a typical OS kernel runs on that. With unikernels, a combined binary of the application and the kernel runs on **ring0**.

Unikernel images would run directly on top of a hypervisor like Xen or on bare metal, based on the unikernel types. The following image shows how the *Mirage Compiler* creates a unikernel VM image.

![Comparison of a Traditional OS Stack and a MirageOS Unikernel](./img/img_0.jpg)

## Unikernels and Docker (MirageOS)

In January of 2016, Docker acquired Unikernels to make them a first-class citizen of the Docker ecosystem. Both containers and unikernels can co-exist on the same host and can be managed by the same Docker binary.

Unikernels helped Docker to run the Docker Engine on top of Alpine Linux on Mac and Windows with their default hypervisors, which are xhyve Virtual Machine and Hyper-V VM respectively.

![Shared Kernel vs. Unikernel](./img/img_1.jpg)
