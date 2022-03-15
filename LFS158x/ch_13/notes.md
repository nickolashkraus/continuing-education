# Chapter 13: ConfigMaps and Secrets

## Tasks
- [x] None

## Introduction

While deploying an application, we may need to pass such runtime parameters like configuration details, permissions, passwords, keys, certificates, or tokens.

Let's assume we need to deploy ten different applications for our customers, and for each customer, we need to display the name of the company in the UI. Then, instead of creating ten different Docker images for each customer, we may just use the same template image and pass customers' names as runtime parameters. In such cases, we can use the **ConfigMap API** resource.

Similarly, when we want to pass sensitive information, we can use the **Secret API** resource. In this chapter, we will explore ConfigMaps and Secrets.

## ConfigMaps

[ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap) allow us to decouple the configuration details from the container image. Using ConfigMaps, we pass configuration data as key-value pairs, which are consumed by Pods or any other system components and controllers, in the form of environment variables, sets of commands and arguments, or volumes. We can create ConfigMaps from literal values, from configuration files, from one or more files or directories.

## Secrets

Let's assume that we have a *Wordpress* blog application, in which our **wordpress** frontend connects to the **MySQL** database backend using a password. While creating the Deployment for **wordpress**, we can include the **MySQL** password in the Deployment's YAML file, but the password would not be protected. The password would be available to anyone who has access to the configuration file.

In this scenario, the [Secret](https://kubernetes.io/docs/concepts/configuration/secret) object can help by allowing us to encode the sensitive information before sharing it. With Secrets, we can share sensitive information like passwords, tokens, or keys in the form of key-value pairs, similar to ConfigMaps; thus, we can control how the information in a Secret is used, reducing the risk for accidental exposures. In Deployments or other resources, the Secret object is *referenced*, without exposing its content.

It is important to keep in mind that by default, the Secret data is stored as plain text inside **etcd**, therefore administrators must limit access to the API server and **etcd**. However, Secret data can be encrypted at rest while it is stored in **etcd**, but this feature needs to be enabled at the API server level.
