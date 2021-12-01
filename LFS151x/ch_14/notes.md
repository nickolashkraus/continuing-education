# Chapter 14: Tools for Cloud Infrastructure III (Key-Value Pair Store)

## Tasks
- [x] None

## Introduction

While building any distributed and dynamically-scalable environment, we need an endpoint which is a single point of truth. For example, we need such an endpoint if we want each node in a distributed environment to look up specific configuration values before performing an operation. For such cases, all the nodes can reach out to a central location and retrieve the value of a desired variable or key.

As the name suggests, the **Key-Value Pair Storage** provides the functionality to store or retrieve the *value* of a *key*. Most of the *Key-Value* stores provide REST APIs to support operations like `GET`, `PUT`, and `DELETE`, which help with operations over **HTTP**. Some examples of *Key-Value* stores are:
* etcd
* Consul
* ZooKeeper

## Introduction to etcd

[etcd](https://etcd.io) is an open source *distributed key-value pair* store. It uses the [Raft consensus algorithm](https://raft.github.io) for communication between instances in distributed multi-instance scenarios. It was started by [CoreOS](https://coreos.com) and it is written in Go. Currently, the etcd project is part of the [CNCF](https://www.cncf.io) family of cloud-native technologies.

## Introduction to Consul

[Consul](https://www.consul.io), of [HashiCorp](https://www.hashicorp.com), is a distributed, highly-available system which can be used for service discovery and configuration.

Other than providing a distributed key-value store, it also provides features like:
* Service discovery in conjunction with DNS or HTTP
* Health checks for services and nodes
* Multi-datacenter support.

While Consul can be configured on a single node, a multi-node configuration is recommended. Consul is built on top of [Serf](https://www.serf.io), which provides membership, failure detection, and event broadcast. Consul also uses [the Raft consensus algorithm](https://raft.github.io) for leadership election and consistency.

## Introduction to ZooKeeper

[ZooKeeper](https://zookeeper.apache.org) is a centralized service for maintaining configuration information, providing distributed synchronization together with group services for distributed applications. It is an open source project under [The Apache Software Foundation](https://www.apache.org).

ZooKeeper aims to provide a simple interface to a centralized coordination service, that is also distributed and highly reliable. It implements Consensus, group management, and presence protocols on behalf of applications.
