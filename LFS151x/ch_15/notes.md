# Chapter 15: Tools for Cloud Infrastructure IV (Image Building)

## Tasks
- [x] None

## Introduction

In an immutable infrastructure environment, we prefer to replace an existing service with a new one, to fix any problems/bugs or to perform an update.

Whether we start a new service or replace an older service with a new one, we need the image from which the service can be started. These images should be created in an automated fashion. In this section, we will take a look into the creation of **Docker** container images, and VM images for different cloud platforms using **Packer**.

## Dockerfiles

We can create a custom Docker image by starting a container from the base image and, after making the required changes (e.g. installing the software), we can use the `docker commit` command to save it to persistent storage. This is not a scalable and efficient solution.

Docker has a feature that allows it to read instructions from a text file and then generate the requested image. Internally, it creates a container after each instruction and then commits it to persistent storage. The file with instructions is referred to as a [Dockerfile](https://docs.docker.com/engine/reference/builder).

Typically, Dockerfiles start from a parent image or a [base image](https://docs.docker.com/develop/develop-images/baseimages), specified with the `FROM` instruction. A parent image is an image used as a reference, modified by subsequent Dockerfile instructions. Parent images can be built directly out of working machines, or with tools such as [Debootstrap](https://wiki.debian.org/Debootstrap). A base image has `FROM scratch` in the Dockerfile.

You may have noticed that we always start with a base image when creating an image from Dockerfile. Someone has to build those images for the first time. There are tools like [Debootstrap](https://wiki.debian.org/Debootstrap) or [supermin](https://github.com/libguestfs/supermin) which can help you build base images.

The [Docker GitHub source code repository](https://github.com/docker/docker/tree/master/contrib) also has helper scripts to create base images.

## Multi-Stage Dockerfile

A [multi-stage build](https://docs.docker.com/develop/develop-images/multistage-build) is a newer feature of Docker, very useful for optimizing the Dockerfiles and minimizing the size of a container image. Through a multi-stage build, we create a new Docker image in every stage. Every stage can copy files from images created either in earlier stages or from already available images.

For example, in the first stage, we can copy the source code, compile it to create a binary, and then, in the second stage, we can copy just the binary to a resulting image. Before multi-stage builds, we would need to create multiple Dockerfiles to achieve similar results.

## Introduction to Packer

[Packer](https://www.packer.io) from [HashiCorp](https://www.hashicorp.com) is an open source tool for creating virtual images from a configuration file for different platforms. Configuration files are written using [HCL](https://github.com/hashicorp/hcl) (HashiCorp Configuration Language).
