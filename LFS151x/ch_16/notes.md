# Chapter 16: Tools for Cloud Infrastructure V (Debugging, Logging, and Monitoring for Containerized Applications)

## Tasks
- [x] None

## Introduction

When we experience problems in development or production, we resort to debugging, logging, and monitoring tools to find the root cause of those problems. Some of the tools we should be familiar with are:
* strace
* SAR (System Activity Reporter)
* tcpdump
* GDB (GNU Project Debugger)
* syslog
* Nagios
* Zabbix

We can use the same tools on bare metal and VMs, but containers bring additional challenges:
* Containers are ephemeral, so, when they are deleted, all their metadata, including logs, gets deleted as well, unless we store it in some other, possibly persistent storage location.
* Containers do not have kernel space components.
* We want to keep a container's footprint as small as possible, but installing debugging and monitoring tools make that nearly impossible.
* Collecting per container statistics, debugging information individually, and then analyzing data from multiple containers is a tedious process.

It would be beneficial to have external tools for logging and monitoring instead of collecting them directly from individual containers. This may be achieved because each container runs as a process on the host OS, which has complete control of that process. Once we have collected the data from all the containers running on a system or in a cluster, like Kubernetes or Swarm, we can run a logical mapping of all collected logs and get a system/cluster-wide view.

Below are some of the tools which we can use for containerized applications:
* **Debugging**: Docker CLI, Sysdig
* **Logging**: Docker CLI, Docker Logging Driver
* **Monitoring**: Docker CLI, Sysdig, cAdvisor, Prometheus, Datadog, New Relic
