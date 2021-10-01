---
title: Quickly deploy OCI images to Kubernetes
excerpt: Quickly deploy OCI images to Kubernetes
date: 2020-04-19 09:32:27
tags:
  - Kubernetes
categories: [Cloud, Kubernetes]
---

# Quickly deploy OCI images to Kubernetes

## Create a ephemeral pod

```shell
# https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#run

kubectl run -it --rm --restart=Never alpine --image=alpine
kubectl run -it --rm --restart=Never centos-7 --image=centos:7
kubectl run -it --rm --restart=Never centos-8 --image=centos:8
kubectl run -it --rm --restart=Never debian-stable --image=debian:stable
kubectl run -it --rm --restart=Never debian-testing --image=debian:testing
kubectl run -it --rm --restart=Never fedora32 --image=fedora:32
kubectl run -it --rm --restart=Never sle15 --image=registry.suse.com/suse/sle15
kubectl run -it --rm --restart=Never ubi7 --image=registry.access.redhat.com/ubi7/ubi
kubectl run -it --rm --restart=Never ubi8 --image=registry.access.redhat.com/ubi8/ubi
kubectl run -it --rm --restart=Never ubuntu-1804 --image=ubuntu:18.04
kubectl run -it --rm --restart=Never ubuntu-2004 --image=ubuntu:20.04
```

## Create a long-lived pod

```shell
kubectl -n kube-system run fedora-32 --generator=run-pod/v1 --image=fedora:32 -- bash -c 'while true; do sleep 10; done'
```

## Attach the long-lived pod

```shell
kubectl -n kube-system exec -it fedora-32 -- bash
```

## Remove the long-lived pod

```shell
kubectl -n kube-system delete pod fedora-32
```
