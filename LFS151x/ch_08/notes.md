# Chapter 8: Microservices

## Tasks
- [x] None

## Introduction

According to [Wikipedia](https://en.wikipedia.org/wiki/Microservices),

>Microservices are small, independent processes that communicate with each other to form complex applications which utilize language-agnostic APIs. These [services](https://en.wikipedia.org/wiki/Service_(systems_architecture)) are small building blocks, highly [decoupled](https://en.wikipedia.org/wiki/Coupling_(computer_programming)) and focused on doing a small task, facilitating a [modular](https://en.wikipedia.org/wiki/Modularity) approach to [system](https://en.wikipedia.org/wiki/System)-building. The microservices architectural style is becoming the standard for building continuously deployed systems.

## Technological Advancement Towards Microservices

Over the last decade, building the right kind of tooling around virtualization and cloud management has accelerated the adoption and consumption of cloud technologies. Below we provide some relevant examples:

* With the launch of [Amazon Web Services](https://aws.amazon.com) (AWS) in 2006, we can get compute resources on demand from the web or the command-line interface.
* With the launch of [Heroku](http://heroku.com) in 2007, we can deploy a locally-built application in the cloud with just a couple of commands.
* With the launch of [Vagrant](https://www.vagrantup.com) in 2010, we can easily create reproducible development environments.

With tools like the ones above in hand, software engineers and architects started to move away from large monolith applications, in which an entire application is managed via one code-base. Having one code-base makes the application difficult to manage and scale.

Over the years, with different experiments, we evolved towards a new approach, in which a single application is deployed and managed via a small set of services. Each service runs its own process and communicates with other services via lightweight mechanisms like REST APIs. Each of these services is independently deployed and managed. Technologies like containers and unikernels are becoming default choices for creating such services.

![Monolithic vs. Microservices](./img/img_0.jpg)

## Refactoring a Monolith Into Microservices

We have been designing applications for decades and have a very good understanding of modularizing the code. In simpler terms, we can create a microservices environment by extending those modules into individual services. Though there is no rule of thumb to follow every time we refactor a monolith into microservices, there are some approaches that we can look at:

* If you have a complex monolith application, then it is not advisable to rewrite the entire application from scratch. Instead, you should start carving out services from the monolith, which implement the desired functionalities for the code we take out from the monolith. Over time, all or most functionalities will be implemented in the microservices architecture.
* We can split the monoliths based on the business logic, front-end (presentation), and data access. In the microservices architecture, it is recommended to have a local database for individual services. And, if the services need to access the database from other services, then we can implement an event-driven communication between these services.
* As mentioned earlier, we can split the monolith based on the modules of the monolith application, and each time we do it, our monolith shrinks.

Also, if we need a new functionality while we are refactoring the monolith to microservices, then we should create a microservice instead of adding more code to the monolith.

## Challenges and Drawbacks of Microservices

Just like any other technology, there are also challenges and disadvantages to using microservices:

**Choosing the right service size**
* While refactoring the monolith application or creating microservices from scratch, it is very important to choose the right functionality for a service. For example, if we create a microservice for each function of a monolith, then we would end up with lots of small services, which would bring unnecessary complexity.

**Deploymen**
* We can easily deploy a monolith application. However, to deploy a microservice, we need to use a distributed environment such as Kubernetes.

**Testing**
* With lots of services and their inter-dependency, sometimes it becomes challenging to perform end-to-end testing of a microservice.

**Inter-service communication**
* Inter-service communication can be very costly if it is not implemented correctly. There are options such as message passing, or RPC and we need to choose the one that fits our requirement and has the least overhead.

**Managing databases**
* When it comes to the microservices' architecture, we may decide to implement a database local to a microservice. But, to close a business loop, we might require changes in other related databases. This can create problems (e.g. partitioned databases).

**Monitoring**
* Monitoring individual services in a microservices environment can be challenging. This challenge is being addressed, and a new set of tools, like [Sysdig](https://sysdig.com) or [Datadog](https://www.datadoghq.com), is being developed to monitor and debug microservices.

Even with the above challenges and drawbacks, deploying microservices makes sense when applications are complex and continuously evolving.
