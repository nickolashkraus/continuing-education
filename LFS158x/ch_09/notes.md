# Chapter 9: Authentication, Authorization, Admission Control

## Tasks
- [x] None

## Introduction

To access and manage Kubernetes resources or objects in the cluster, we need to access a specific API endpoint on the API server. Each access request goes through the following access control stages:

**Authentication**
* Logs in a user.

**Authorization**
* Authorizes the API requests submitted by the authenticated user.

**Admission Control**
* Software modules that validate and/or modify user requests.

## Authentication

Kubernetes does not have an object called *user*, nor does it store *usernames* or other related details in its object store. However, even without that, Kubernetes can use usernames for the [Authentication](https://kubernetes.io/docs/reference/access-authn-authz/authentication) phase of the API access control, and to request logging as well.

Kubernetes supports two kinds of [users](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#users-in-kubernetes):

**Normal Users**
* They are managed outside of the Kubernetes cluster via independent services like User/Client Certificates, a file listing usernames/passwords, Google accounts, etc.

**Service Accounts**
* Service Accounts allow in-cluster processes to communicate with the API server to perform various operations. Most of the Service Accounts are created automatically via the API server, but they can also be created manually. The Service Accounts are tied to a particular Namespace and mount the respective credentials to communicate with the API server as Secrets.

If properly configured, Kubernetes can also support [anonymous requests](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#anonymous-requests), along with requests from Normal Users and Service Accounts. [User impersonation](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#user-impersonation) is also supported allowing a user to act as another user, a helpful feature for administrators when troubleshooting authorization policies.

For authentication, Kubernetes uses a series of [authentication modules](https://kubernetes.io/docs/reference/access-authn-authz/authentication/#authentication-strategies):

**X509 Client Certificates**
* To enable client certificate authentication, we need to reference a file containing one or more certificate authorities by passing the `--client-ca-file=SOMEFILE` option to the API server. The certificate authorities mentioned in the file would validate the client certificates presented by users to the API server. A demonstration video covering this topic can be found at the end of this chapter.

**Static Token File**
* We can pass a file containing pre-defined bearer tokens with the `--token-auth-file=SOMEFILE` option to the API server. Currently, these tokens would last indefinitely, and they cannot be changed without restarting the API server.

**Bootstrap Tokens**
* Tokens used for bootstrapping new Kubernetes clusters.

**Service Account Tokens**
* Automatically enabled authenticators that use signed bearer tokens to verify requests. These tokens get attached to Pods using the ServiceAccount Admission Controller, which allows in-cluster processes to talk to the API server.

**OpenID Connect Tokens**
* OpenID Connect helps us connect with OAuth2 providers, such as Azure Active Directory, Salesforce, and Google, to offload the authentication to external services.

**Webhook Token Authentication**
* With Webhook-based authentication, verification of bearer tokens can be offloaded to a remote service.

**Authenticating Proxy**
* Allows for the programming of additional authentication logic.

We can enable multiple authenticators, and the first module to successfully authenticate the request short-circuits the evaluation. To ensure successful user authentication, we should enable at least two methods: the service account tokens authenticator and one of the user authenticators.

## Authorization

After a successful authentication, users can send the API requests to perform different operations. Here, these API requests get [authorized](https://kubernetes.io/docs/reference/access-authn-authz/authorization) by Kubernetes using various authorization modules, that allow or deny the requests.

Some of the API request attributes that are reviewed by Kubernetes include user, group, extra, Resource, Namespace, or API group, to name a few. Next, these attributes are evaluated against policies. If the evaluation is successful, then the request is allowed, otherwise it is denied. Similar to the Authentication step, Authorization has multiple modules, or authorizers. More than one module can be configured for one Kubernetes cluster, and each module is checked in sequence. If any authorizer approves or denies a request, then that decision is returned immediately.

Authorization modes:

**Node**
* Node authorization is a special-purpose authorization mode which specifically authorizes API requests made by kubelets. It authorizes the kubelet's read operations for Services, Endpoints, or Nodes, and writes operations for Nodes, Pods, and Events. For more details, please review the [Node authorization](https://kubernetes.io/docs/reference/access-authn-authz/node).

**Attribute-Based Access Control (ABAC)**
* With the ABAC authorizer, Kubernetes grants access to API requests, which combine policies with attributes. In the following example, user *student* can only read Pods in the Namespace **lfs158**.

```yaml
{
  "apiVersion": "abac.authorization.kubernetes.io/v1beta1",
  "kind": "Policy",
  "spec": {
    "user": "student",
    "namespace": "lfs158",
    "resource": "pods",
    "readonly": true
  }
}
```

To enable ABAC mode, we start the API server with the `--authorization-mode=ABAC` option, while specifying the authorization policy with `--authorization-policy-file=PolicyFile.json`. For more details, please review the [ABAC authorization](https://kubernetes.io/docs/reference/access-authn-authz/abac).

**Webhook**
* In Webhook mode, Kubernetes can request authorization decisions to be made by third-party services, which would return *true* for successful authorization, and *false* for failure. In order to enable the Webhook authorizer, we need to start the API server with the `--authorization-webhook-config-file=SOME_FILENAME` option, where `SOME_FILENAME` is the configuration of the remote authorization service. For more details, please see the [Webhook mode](https://kubernetes.io/docs/reference/access-authn-authz/webhook).

**Role-Based Access Control (RBAC)**
* In general, with RBAC we regulate the access to resources based on the Roles of individual users. In Kubernetes, multiple Roles can be attached to subjects like users, service accounts, etc. While creating the Roles, we restrict resource access by specific operations, such as `create`, `get`, `update`, `patch`, etc. These operations are referred to as verbs.

In RBAC, we can create two kinds of Roles:

**Role**
* A Role grants access to resources within a specific Namespace.

**ClusterRole**
* A ClusterRole grants the same permissions as Role does, but its scope is cluster-wide.

In this course, we will focus on the first kind, **Role**. Below you will find an example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: lfs158
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
```

The manifest defines a bind between the **pod-reader** Role and the **student** user, to restrict the user to only read the Pods of the **lfs158** Namespace.

There are two kinds of RoleBindings:

**RoleBinding**
* It allows us to bind users to the same namespace as a Role. We could also refer a ClusterRole in RoleBinding, which would grant permissions to Namespace resources defined in the ClusterRole within the RoleBinding’s Namespace.

**ClusterRoleBinding**
* It allows us to grant access to resources at a cluster-level and to all Namespaces.

In this course, we will focus on the first kind, **RoleBinding**. Below, you will find an example:

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
- kind: User
  name: student
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
```

The manifest defines a bind between the **pod-reader** Role and the **student** user, to restrict the user to only read the Pods of the **lfs158** Namespace.

To enable the RBAC mode, we start the API server with the `--authorization-mode=RBAC` option, allowing us to dynamically configure policies. For more details, please review the [RBAC mode](https://kubernetes.io/docs/reference/access-authn-authz/rbac).

## Admission Control

[Admission Controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers) are used to specify granular access control policies, which include allowing privileged containers, checking on resource quota, etc. We force these policies using different admission controllers, like ResourceQuota, DefaultStorageClass, AlwaysPullImages, etc. They come into effect only after API requests are authenticated and authorized.

To use admission controls, we must start the Kubernetes API server with the **—enable-admission-plugins**, which takes a comma-delimited, ordered list of controller names:

```
--enable-admission-plugins=NamespaceLifecycle,ResourceQuota,PodSecurityPolicy,DefaultStorageClass
```

Kubernetes has some admission controllers enabled by default. For more details, please review the [list of Admission Controllers](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#what-does-each-admission-controller-do).

Kubernetes admission control can also be implemented though custom plugins, for a [Dynamic Admission Control](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers) method. These plugins are developed as extensions and run as admission webhooks.
