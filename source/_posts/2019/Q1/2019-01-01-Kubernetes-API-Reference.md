---
title: Kubernetes API Reference
excerpt: Distributed System Design
date: 2019-01-01 09:57:04
tags:
  - Cloud
  - Kubernetes
categories: [Cloud, Kubernetes]
---

# Kubernetes API Reference

You can use the Kubernetes API to read and write Kubernetes resource objects via a Kubernetes API endpoint.

- https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.12/
- https://kubernetes.io/docs/setup/independent/install-kubeadm/
- https://kubernetes.io/docs/setup/independent/create-cluster-kubeadm/
- https://kubernetes.io/docs/setup/independent/ha-topology/
- https://kubernetes.io/docs/setup/independent/high-availability/
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/

## API overview

### Resource Categories

This is a high-level overview of the basic types of resources provide by the Kubernetes API and their primary functions.

- **Workloads** are objects you use to manage and run your containers on the cluster.
- **Discovery & LB** resources are objects you use to "stitch" your workloads together into an externally accessible, load-balanced Service.
- **Config & Storage** resources are objects you use to inject initialization data into your applications, and to persist data that is external to your container.
- **Cluster** resources objects define how the cluster itself is configured; these are typically used only by cluster operators.
- **Metadata** resources are objects you use to configure the behavior of other resources within the cluster, such as HorizontalPodAutoscaler for scaling workloads.

### Resource Objects
Resource objects typically have 3 components:

- **Resource ObjectMeta**: This is metadata about the resource, such as its name, type, api version, annotations, and labels. This contains fields that maybe updated both by the end user and the system (e.g. annotations).
- **ResourceSpec**: This is defined by the user and describes the desired state of system. Fill this in when creating or updating an object.
- **ResourceStatus**: This is filled in by the server and reports the current state of the system. In most cases, users don't need to change this.

### Resource Operations
Most resources provide the following Operations:

- Create
    Create operations will create the resource in the storage backend. After a resource is create the system will apply the desired state.
- Update
    Updates come in 2 forms: **Replace** and **Patch**:
    - **Replace**: Replacing a resource object will update the resource by replacing the existing spec with the provided one.
    - **Patch**: Patch will apply a change to a specific field. How the change is merged is defined per field. Lists may either be replaced or merged. Merging lists will not preserve ordering.
- Read
    **Reads** come in 3 forms: **Get**, **List** and **Watch**:
    - **Get**: Get will retrieve a specific resource object by name.
    - **List**: List will retrieve all resource objects of a specific type within a namespace, and the results can be restricted to resources matching a selector query.
      **List All Namespaces**: Like List but retrieves resources across all namespaces.
    - **Watch**: Watch will stream results for an object(s) as it is updated. Similar to a callback, watch is used to respond to resource changes.
- Delete
    **Delete** will delete a resource. Depending on the specific resource, child objects may or may not be garbage collected by the server. See notes on specific resource objects for details.
- Additional Operations
    Resources may define additional operations specific to that resource type.

    - **Rollback**: Rollback a PodTemplate to a previous version. Only available for some resource types.
    - **Read / Write Scale**: Read or Update the number of replicas for the given resource. Only available for some resource types.
    - **Read / Write Status**: Read or Update the Status for a resource object. The Status can only changed through these update operations.

## Workloads APIs

- Container v1 core
- CronJob v1beta1 batch
- DaemonSet v1 apps
- Deployment v1 apps
- Job v1 batch
- Pod v1 core
- ReplicaSet v1 apps
- ReplicationController v1 core
- StatefulSet v1 apps

## Service APIs

- Endpoints v1 core
- Ingress v1beta1 extensions
- Service v1 core

## Config and Storage APIs

- ConfigMap v1 core
- Secret v1 core
- PersistentVolumeClaim v1 core
- StorageClass v1 storage.k8s.io
- Volume v1 core
- VolumeAttachment v1beta1 storage.k8s.io

## Metadata APIs

- Event v1 core
- LimitRange v1 core
- PodTemplate v1 core
- HorizontalPodAutoscaler v1 autoscaling
- ...

## Cluster APIs

- Binding v1 core
- ClusterRole v1 rbac.authorization.k8s.io
- ClusterRoleBinding v1 rbac.authorization.k8s.io
- Namespace v1 core
- Node v1 core
- PersistentVolume v1 core
- ResourceQuota v1 core
- Role v1 rbac.authorization.k8s.io
- RoleBinding v1 rbac.authorization.k8s.io
- ServiceAccount v1 core
- ...
