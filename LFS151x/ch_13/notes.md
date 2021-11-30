# Chapter 13: Tools for Cloud Infrastructure II (Build & Release)

## Tasks
- [x] None

## Introduction

With source code management tools like Git, we can easily version the code and retrieve the same bits we saved in the past. This saves a lot of time and helps developers automate most of the non-coding activities, like creating automated builds, running tests, etc. Extending the same analogy to infrastructure would allow us to create a reproducible deployment environment, which is referred to as Infrastructure as a Code.

Infrastructure as a Code helps us create a near production-like environment for development, staging, etc. With some tooling around them, we can also create the same environments on different cloud providers.

By combining Infrastructure as a Code with versioned software, we are guaranteed to have a reproducible build and release environment every time. In this chapter, we will take a look at such tools: **Terraform**, **CloudFormation**, and **BOSH**.

## Introduction to Terraform

[Terraform](https://www.terraform.io) is a tool that allows us to define the infrastructure as code. This helps us deploy the same infrastructure on Virtual Machines, bare metal, or cloud. It helps us treat the infrastructure as software. The configuration files can be written in [HCL](https://github.com/hashicorp/hcl) (HashiCorp Configuration Language).

## Terraform Providers

Physical machines, VMs, network switches, or containers are treated as resources, which are exposed by providers.

A provider is responsible for understanding API interactions and exposing resources, which makes Terraform agnostic to the underlying platforms. A custom provider can be created through plugins.

## Introduction to CloudFormation

[CloudFormation](https://aws.amazon.com/cloudformation) is a tool that allows us to define our infrastructure as code on Amazon AWS. This helps us treat our AWS infrastructure as software. The configuration files can be written in YAML or JSON format, while CloudFormation can also be used from a web console or from the command line.

## Introduction to BOSH

According to [bosh.io](https://bosh.io),

>"BOSH is an open source tool for release engineering, deployment, lifecycle management, and monitoring of distributed systems".

**BOSH** was primarily developed to deploy the Cloud Foundry PaaS, but it can deploy other software as well (e.g. Hadoop). BOSH supports multiple Infrastructure as a Service (IaaS) providers. BOSH creates VMs on top of IaaS, configures them to suit the requirements, and then deploys the applications on them. Supported IaaS providers for BOSH are:
* Amazon Web Services EC2
* OpenStack
* VMware vSphere
* vCloud Director

With the Cloud Provider Interface (CPI), BOSH supports additional IaaS providers such as [Google Compute Engine](https://github.com/cloudfoundry/bosh-google-cpi-release) and [Apache CloudStack](https://github.com/orange-cloudfoundry/bosh-cloudstack-cpi-release).
