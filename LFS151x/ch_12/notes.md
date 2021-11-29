# Chapter 12: Tools for Cloud Infrastructure I (Configuration Management)

## Tasks
- [x] None

## Introduction

When we have numerous systems (both bare-metal and virtual) to manage in different environments like Development, QA, and Production, we would prefer to automate it. At any point in time, we want to have a consistent and desired state of systems and software installed on them. This is also referred to as **Infrastructure as Code**.

**Configuration Management** tools allow us to define the desired state of the systems in an automated way. In this section, we will take a look at some of the Configuration Management tools available today, such as Ansible, Chef, Puppet, and Salt.

## Introduction to Ansible

Red Hat's [Ansible](http://www.ansible.com) is an easy-to-use, open source Configuration Management (CM) tool. It is an agentless tool that works through SSH. In addition, Ansible can automate infrastructure provisioning (on-premise or cloud), application deployment, and orchestration.

Due to its simplicity, Ansible quickly became one of the top in-demand CM tools in the DevOps world.

## Introduction to Puppet

[Puppet](https://puppet.com) is an open source configuration management tool. It mostly uses the agent/master (client/server) model to configure the systems. The agent is referred to as the Puppet Agent and the master is referred to as the Puppet Master. The Puppet Agent can also work locally and is then referred to as Puppet Apply.

Besides Puppet, the company also has an enterprise product called [Puppet Enterprise](https://puppet.com/products/puppet-enterprise) and provides services and training for this product as well.

## Introduction to Chef

[Chef](https://www.chef.io) uses the client/server model to perform configuration management. The client is installed on each host which we want to manage and is referred to as [Chef Client](https://docs.chef.io/chef_client_overview). The server is referred to as [Chef Server](https://docs.chef.io/server_overview). Additionally, there is another component called [Chef Workstation](https://docs.chef.io/workstation), which is used to:
* Develop [cookbooks](https://docs.chef.io/chef_overview/#cookbooks) and [recipes](https://docs.chef.io/recipes)
* Synchronize [chef-repo](https://docs.chef.io/chef_repo) with the version control system
* Run command-line tools
* Configure [policy](https://docs.chef.io/policy), [roles](https://docs.chef.io/roles), etc.
* Interact with nodes to perform a one-off configuration

## Introduction to Salt Open

[Salt Open](https://www.saltstack.com/open-source-projects), or simply **Salt**, is an open source configuration management system built on top of a remote execution framework. It can be used in a client/server model or agentless model as well. In a client/server model the server sends commands and configurations to all the clients in a parallel manner, which the clients run, returning back the status. In the agentless mode, the server connects with remote systems via SSH.

[SaltStack](https://www.saltstack.com), which stands behind **Salt Open**, offers also an enterprise product called [SaltStack Enterprise](https://www.saltstack.com/products/saltstack-enterprise).
