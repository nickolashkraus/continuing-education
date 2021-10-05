# Chapter 1: The Linux Foundation

## Tasks
- [x] None

## About The Linux Foundation

[The Linux Foundation](https://www.linuxfoundation.org) provides a neutral, trusted hub for developers to code, manage, and scale open technology projects.

## Focus on Three Major Linux Distribution Families

The families and representative distributions we are using are:
* **Red Hat Family Systems**: CentOS, CentOS Stream, Fedora and Oracle Linux
* **SUSE Family Systems**: SUSE (SUSE Linux Enterprise Server or SLES) and openSUSE
* **Debian Family Systems**: Debian, Ubuntu and Linux Mint

![The Linux Kernel Distribution Families and Individual Distributions](./img/img_0.png)

For a comprehensive list of available distributions, see [The LWN.net Linux Distribution List](https://lwn.net/Distributions).

## The Red Hat Family

Red Hat Enterprise Linux (RHEL) heads the family that includes CentOS, CentOS Stream, Fedora and Oracle Linux.

Fedora has a close relationship with RHEL and contains significantly more software than Red Hat's enterprise version. One reason for this is that a diverse community is involved in building Fedora, with many contributors who do not work for Red Hat. Furthermore, it is used as a testing platform for future RHEL releases.

CentOS Stream and CentOS have no cost to the end user and there is a longer release cycle than for Fedora (which releases a new version every six months or so).

The basic version of CentOS is also virtually identical to RHEL, the most popular Linux distribution in enterprise environments. However, CentOS 8 has no scheduled updates after 2021. The replacement is CentOS 8 Stream. The difference between the two versions is CentOS Stream gets updates before RHEL, while CentOS gets them after.

**Key Facts**

* Fedora serves as an upstream testing platform for RHEL.
* CentOS is a close clone of RHEL, while Oracle Linux is mostly a copy with some changes (in fact, CentOS has been part of Red Hat since 2014).
* A heavily patched version 3.10 kernel is used in RHEL/CentOS 7, while version 4.18 is used in RHEL/CentOS 8.
* It supports hardware platforms such as Intel x86, Arm, Itanium, PowerPC, and IBM System z.
* It uses the yum and dnf RPM-based yum package managers to install, update, and remove packages in the system.
* RHEL is widely used by enterprises which host their own systems.

## The SUSE Family

The relationship between SUSE (SUSE Linux Enterprise Server, or SLES) and openSUSE is similar to the one described between RHEL, CentOS, and Fedora.

openSUSE is available to end users at no cost. Because the two products are extremely similar, material that covers openSUSE can typically be applied to SLES with few problems.

**Key Facts**

* SUSE Linux Enterprise Server (SLES) is upstream for openSUSE.
* Kernel version 4.12 is used in openSUSE Leap 15.
* It uses the RPM-based zypper package manager to install, update, and remove packages in the system.
* It includes the YaST (Yet Another Setup Tool) application for system administration purposes.
* SLES is widely used in retail and many other sectors.

## The Debian Family

The Debian distribution is upstream for several other distributions, including Ubuntu. In turn, Ubuntu is upstream for Linux Mint and a number of other distributions. It is commonly used on both servers and desktop computers. Debian is a pure open source community project (not owned by any corporation) and has a strong focus on stability.

Debian provides by far the largest and most complete software repository to its users of any Linux distribution.

Ubuntu aims at providing a good compromise between long-term stability and ease of use. Since Ubuntu gets most of its packages from Debian’s stable branch, it also has access to a very large software repository.

**Key Facts**

* The Debian family is upstream for Ubuntu and Ubuntu is upstream for Linux Mint and others.
* Kernel version 5.8 is used in Ubuntu 20.04 LTS.
* It uses the DPKG-based APT package manager (using apt, apt-get, apt-cache, etc.) to install, update, and remove packages in the system.
* Ubuntu has been widely used for cloud deployments.
* While Ubuntu is built on top of Debian and is GNOME-based under the hood, it differs visually from the interface on standard Debian, as well as other distributions.
