# Chapter 10: Software-Defined Storage and Storage Management for Containers

## Tasks
- [x] None

## Introduction

**Software-Defined Storage** (**SDS**) represents storage virtualization in which the underlying storage hardware is separated from the software that manages and provisions it. We can combine physical hardware from various sources and manage them with software, as a single storage pool. SDS replaces static and inefficient storage solutions backed directly by physical hardware with dynamic, agile, and automated solutions. In addition, SDS may provide resiliency features such as replication, erasure coding, and snapshots of the pooled resources. Once the pooled storage is configured in the form of a storage cluster, SDS allows multiple access methods such as *File*, *Block*, and *Object*.

Some examples of Software-Defined Storage are:
* [Ceph](https://ceph.io)
* [FreeNAS](https://www.freenas.org)
* [Gluster](https://www.gluster.org)
* [LINBIT](https://www.linbit.com)
* [MinIO](https://min.io)
* [Nexenta](https://nexenta.com)
* [OpenEBS](https://openebs.io)
* [OpenSDS](https://github.com/sodafoundation/api/blob/master/OpenSDS%20Architecture.jpg)
* [Soda Dock](https://sodafoundation.io/projects/soda-dock)
* [VMware vSAN](https://www.vmware.com/products/vsan.html)

![Software-Defined Storage (SDS)](./img/img_0.jpg)

## Introduction to Storage Management for Containers

Containers are ephemeral in nature, meaning that all data stored inside the container's file system would be lost when the container is deleted. It is best practice to store data outside the container, which keeps the data accessible even after the container is deleted.

In a multi-host or clustered environment, containers can be scheduled to run on any host. We need to make sure the data volume required by the container is available on the host on which the container is scheduled to run.

## Docker Storage Backends

Docker uses the [copy-on-write](https://en.wikipedia.org/wiki/Copy-on-write) mechanism when containers are started from container images. The container image is protected from direct edits by being saved on a read-only filesystem layer. All of the changes performed by the container to the image filesystem are saved on a writable filesystem layer of the container. Docker images and containers are stored on the host system and we can choose the storage backend for Docker storage, depending on our requirements. Docker supports the following storage backends on Linux:

* AUFS (Another Union File System)
* BtrFS
* Device Mapper
* Overlay
* VFS (Virtual File System)
* ZFS

## Volume Management in Kubernetes

Kubernetes uses volumes to attach external storage to containers managed by Pods. A volume is essentially a directory, backed by a storage medium. The storage medium and its contents are determined by the volume type.

A volume in Kubernetes is linked to a Pod and shared among containers of that Pod. The volume has the same lifetime as the Pod but it outlives the containers of that Pod, meaning that data remains preserved across container restarts. However, once the Pod is deleted, the volume and all its data are lost as well.

A volume may be shared by some of the containers running in the same Pod. The diagram above illustrates a Pod with two containers, a File Puller and a Web Server, sharing a storage Volume.

## Volume Types

A volume mounted inside a Pod is backed by an underlying volume type. A volume type decides the properties of the volume, such as size and content type. Some of the [volume types](https://kubernetes.io/docs/concepts/storage/volumes/#types-of-volumes) supported by Kubernetes are:

**configMap**
* To attach a decoupled object that includes configuration data, scripts, and possibly entire filesystems, to containers of a Pod.

**emptyDir**
* An empty volume is created for the Pod as soon as it is scheduled on a worker node. The life of the volume is tightly coupled with the Pod. If the Pod dies, the content of **emptyDir** is deleted forever.

**hostPath**
* To share a directory from the host with the containers of a Pod. If the Pod dies, the content of the volume is still available on the host.

**persistentVolumeClaim**
* To attach a persistent volume to a Pod. Persistent Volumes are covered in the next section.

**secret**
* To attach sensitive information such as passwords, keys, certificates, or tokens to containers in a Pod.

## Persistent Volumes

In a typical IT environment, storage is managed by the storage/system administrators. The end-user gets instructions to use the storage and he/she does not have to worry about the underlying storage management.

In the containerized world, we would like to follow similar rules, but it becomes very challenging given the many volume types we have seen earlier. In Kubernetes, this problem is solved with the persistent volume subsystem, which provides APIs to manage and consume the storage. To manage the volume, Kubernetes uses the [PersistentVolume (PV)](https://kubernetes.io/docs/concepts/storage/persistent-volumes) resource type and to consume it, uses the [PersistentVolumeClaim (PVC)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) resource type.

Persistent volumes can be provisioned statically or dynamically.

For dynamic provisioning of PVs, Kubernetes uses the *StorageClass* resource, which contains pre-defined provisioners and parameters for the PV creation. With *PersistentVolumeClaim* (PVC), a user sends the requests for dynamic PV creation, which gets wired to the *StorageClass* resource.

Some of the [volume types](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes) that support managing storage using PersistentVolume are:

* GCEPersistentDisk
* AWSElasticBlockStore
* AzureFile
* AzureDisk
* NFS
* iSCSI
* RBD
* CephFS
* GlusterFS
* VsphereVolume
* StorageOS

## Persistent Volumes Claim

A [PersistentVolumeClaim (PVC)](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#types-of-persistent-volumes) is a request for storage by a user. Users request for PV resources based on size, access modes, and volume type. Once a suitable PV is found, it is bound to PVC:

After a successful bind, the PVC can be used in a Pod, to allow the containers' access to the PV.

Once a user completed his/her tasks and the Pod is deleted, the PVC may be detached from the PV releasing it for possible future use. Keep in mind, however, that the PVC may be detached from the PV once all the Pods using the same PVC have completed their activities and have been deleted. Once released, the PV can be either deleted, retained, or recycled for future usage, all based on the [reclaim policy](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#reclaim-policy) the user has configured on the PV.

## Container Storage Interface (CSI)

For a while, container orchestrators like Kubernetes, Mesos, Docker, and Cloud Foundry have had their specific methods of managing external storage volumes. As a result, for storage vendors, it was difficult to manage different volume plugins for different orchestrators. Storage vendors and community members from different orchestrators have been working together to standardize the volume interface to avoid duplicate work. This is the aim of the ** (CSI)** [Container Storage Interface](https://github.com/container-storage-interface/spec), that the same volume plugin would work with different container orchestrators out of the box.

Such interoperability may only be achieved through an industry standard to be implemented by all storage providers which are developing universal plugins expected to work with all container orchestrators. The role of CSI is to maintaining the CSI [specification](https://github.com/container-storage-interface/spec/blob/master/spec.md) and the [protobuf](https://github.com/container-storage-interface/spec/blob/master/csi.proto).

The goal of the CSI specification is to define APIs for dynamic provisioning, attaching, mounting, consumption, and snapshot management of storage volumes. In addition, it defines the plugin configuration steps to be taken by the container orchestrator together with deployment configuration options.
