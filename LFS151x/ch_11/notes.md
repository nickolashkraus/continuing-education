# Chapter 11. DevOps and CI/CD

## Tasks
- [x] None

## Introduction

Every industry thrives for better quality and faster innovation. The IT industry is no exception and has to address numerous challenges:

* It must quickly go from business idea to market.
* It must lower the failure rate for new releases.
* It must have a shorter lead time between fixes.
* It must have a faster mean time to recovery.

Over the last decade or so, we gradually shifted from the [Waterfall model](https://en.wikipedia.org/wiki/Waterfall_model) to [Agile software development](https://en.wikipedia.org/wiki/Agile_software_development), in which teams deliver working software in smaller and more frequent increments. In this process, the IT operations (Ops) teams were unintentionally left behind, which put a lot of pressure on them, due to high end-to-end deployment rates. By putting the Ops teams in the loop from the very beginning of the development cycle, we can reduce this burn-out and can take advantage of their expertise in the continuous integration process.

The collaborative work between Developers and Operations is referred to as **DevOps**. DevOps is more of a mindset, a way of thinking, versus a set of processes implemented in a specific way.

Besides **Continuous Integration** (**CI**), DevOps also enables **Continuous Deployment** (**CD**), which can be seen as the next step of CI. In CD, we deploy the entire application/software automatically, provided that all the tests' results and conditions have met the expectations.

## Introduction to Jenkins

[Jenkins](https://www.jenkins.io) is one of the most popular automation tools, part of the [CD Foundation](https://cd.foundation). It is an open source automation system that can provide Continuous Integration (CI) and Continuous Deployment (CD), and it is written in Java.

## Introduction to Travis CI

[Travis CI](https://travis-ci.com) is a hosted, distributed CI solution for projects hosted on GitHub, Bitbucket, and more.

## Introduction to Shippable

As per the [Shippable](http://docs.shippable.com) website:

>"Shippable is a DevOps Assembly Lines Platform that helps developers and DevOps teams achieve CI/CD and make software releases frequent, predictable, and error-free. We do this by connecting all your DevOps tools and activities into a event-driven, stateful workflow."

## Concourse

[Concourse](https://concourse-ci.org) is an open source CI/CD system, which was started by Alex Suraci and Christopher Brown in 2014. Later, Pivotal sponsored the project. It is written in the Go language.

## Cloud Native CI/CD

So far, we have seen that containers are now playing a major role in an application's lifecycle, from packaging to deployment. Containers have brought Dev, QA, and Ops teams together, which led to a significant improvement in CI/CD. We can code our CI/CD pipeline and store it along with the source code as we've seen in the earlier sections.

We have also seen various methods to run and manage containers at scale using container orchestrators. There are many options for orchestrators, but Kubernetes seems to be the preferred orchestration tool.

In the Cloud Native approach, we design a package and run applications on top of our infrastructure (on-premise or cloud) using operations tools like containers, container orchestration, and services like continuous integration, logging, monitoring, etc. Kubernetes integrated with several tools meets our requirements to run Cloud Native applications.

## Tools

We have already looked at Kubernetes in earlier chapters. Let’s now look at some of the tools that help us with CI/CD, which integrate well with Kubernetes for Cloud Native applications:

* [Helm](https://www.helm.sh) It is a popular package manager for Kubernetes. Helm packages Kubernetes applications into Charts, with all the artifacts, objects, and dependencies an entire application needs in order to successfully be deployed in a Kubernetes cluster. Using Helm Charts, which are stored in repositories, we can share, install, upgrade, or rollback an application that was built to run on Kubernetes. Helm is a graduated project of [CNCF](https://www.cncf.io).

* [Draft](https://draft.sh) It is being promoted as a developer tool for cloud-native applications running on Kubernetes, and it allows for an application to be containerized and deployed on Kubernetes.

* [Skaffold](https://skaffold.dev) It is a tool from Google that helps us build, push, and deploy code to the Kubernetes cluster. It supports *Helm*, and it also provides building blocks and describes customizations for a CI/CD pipeline.

* [Argo](https://argoproj.github.io) It is a container-native workflow engine for Kubernetes. Its use cases include running Machine Learning, Data Processing, and CI/CD tasks on Kubernetes.

* [Jenkins X](https://jenkins-x.io) Jenkins is a very popular tool for CI/CD that can be used on Kubernetes as well. But the Jenkins team built a new cloud-native CI/CD tool, Jenkins X, from the ground up. The new tool leverages Docker, Draft, and Helm to deploy a Jenkins CI/CD pipeline directly on Kubernetes, by simplifying and automating full CI/CD pipelines. In addition, Jenkins X automates the preview of pull requests for fast feedback before changes are merged, and then it automates the environment management and the promotion of new application versions between different environments. Jenkins X is a graduated project of [CD Foundation](https://cd.foundation).

* [Spinnaker](https://www.spinnaker.io) It is an open source multi-cloud continuous delivery platform from Netflix for releasing software changes with high velocity. It supports all the major cloud providers like Amazon Web Services, Microsoft Azure, Google Cloud Platform, and OpenStack. It supports Kubernetes natively. Spinnaker is a graduated project of [CD Foundation](https://cd.foundation).

## GitOps, SecOps, NetOps, and NetSecOps

While we are talking about CI/CD, let's talk about GitOps and SecOps practices as well, which have emerged recently. So far, we have been using Git as a single point of truth for code commits. With GitOps, it is extended to the entire system. Git becomes the single point of truth for code, deployment configuration, monitoring rules, etc. With just a Git pull request, we can do/update the complete application deployment. Some of the examples of GitOps are Weave Flux and GitKube.

With SecOps, security would not be an afterthought when it comes to application deployment. It would be included along with the application deployment, to avoid any security risks. In addition to security, networking and its security also become part of Operations, as NetOps and NetSecOps. Software-defined networking blends right into DevOps practices and allows network and security teams to automate many operational steps by utilizing a policy-driven framework to drive the configuration and operation of the infrastructure.
