# Chapter 20: Distributed Tracing

## Tasks
- [x] None

## Introduction

Organizations are adopting microservices for agility, easy deployment, scaling, etc. However, with a large number of services working together, sometimes it becomes difficult to pinpoint the root cause of latency issues or unexpected behaviors. To overcome such situations, we would need to instrument the behavior of each participating service in our microservices-based application. After collecting and combining the instrumented data from each participating service, we should be able to gain visibility of the entire system. This is generally referred to as **distributed tracing**.

There are various distributed tracing tools like Zipkin, Dapper, HTrace, and X-Trace, but they instrument applications using their own specific APIs, which are not compatible with each other. Due to this tight coupling, developers do not feel very comfortable with them. Enter [OpenTracing](http://opentracing.io), which offers consistent, expressive, vendor-neutral APIs. OpenTracing is an open source project under [CNCF](https://cncf.io).

## Tracing with OpenTracing

In general, a trace represents the details about a transaction, like how much time it took to call a specific function. In OpenTracing, a trace is referred by the directed acyclic graph (DAG) of [spans](https://opentracing.io/specification/#the-opentracing-data-model). Each span can be referred to as a timed operation between contiguous segments of work. In distributed tracing, each service would contribute to its own span or set of spans. A parent can start other spans, either in serial or in parallel. A general tracing for microservices-based applications looks something like the following:

![A General Tracing for Microservices-based Applications](./img/img_0.jpg)

However, it does not give us timing information and is difficult to understand when parallelism is involved. On the other hand, the following visualization adds the context of time, sets up service hierarchy, and demonstrates serial and parallel execution.

![Visualization of a Trace Collected via OpenTracing](./img/img_1.jpg)

We collect information to create graphs like the one above using different [tracers](https://opentracing.io/docs/supported-tracers).

## Introduction to Jaeger

[Jaeger](https://jaegertracing.io) is an open source tracer which is compatible with the OpenTracing data model to support spans. Jaeger was open sourced by [Uber Technologies](https://uber.github.io) and is now part of CNCF. It can be used for the following:
* Distributed content propagation
* Distributed transaction monitoring
* Root cause analysis
* Service dependency analysis
* Performance and latency optimization.
