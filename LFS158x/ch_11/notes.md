# Chapter 11: Deploying a Stand-Alone Application

## Tasks
- [x] None

## Introduction

In this chapter, we will learn how to deploy an application using the Dashboard (Kubernetes WebUI) and the Command Line Interface (CLI). We will also expose the application with a NodePort type Service, and access it from outside the Minikube cluster.

## Liveness and Readiness Probes

While containerized applications are scheduled to run in pods on nodes across our cluster, at times the applications may become unresponsive or may be delayed during startup. Implementing [Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes) allows the **kubelet** to control the health of the application running inside a Pod's container and force a container restart of an unresponsive application. When defining both **Readiness** and **Liveness Probes**, it is recommended to allow enough time for the **Readiness Probe** to possibly fail a few times before a pass, and only then check the **Liveness Probe**. If **Readiness** and **Liveness Probes** overlap there may be a risk that the container never reaches ready state, being stuck in an infinite re-create - fail loop.

## Liveness

If a container in the Pod has been running successfully for a while, but the application running inside this container suddenly stopped responding to our requests, then that container is no longer useful to us. This kind of situation can occur, for example, due to application deadlock or memory pressure. In such a case, it is recommended to restart the container to make the application available.

Rather than restarting it manually, we can use a **Liveness Probe**. Liveness probe checks on an application’s health, and if the health check fails, **kubelet** restarts the affected container automatically.

Liveness Probes can be set by defining:
* Liveness command
* Liveness HTTP request
* TCP Liveness probe

## Readiness Probes

Sometimes, while initializing, applications have to meet certain conditions before they become ready to serve traffic. These conditions include ensuring that the depending service is ready, or acknowledging that a large dataset needs to be loaded, etc. In such cases, we use [Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes) and wait for a certain condition to occur. Only then, the application can serve traffic.

A Pod with containers that do not report ready status will not receive traffic from Kubernetes Services.
