---
title: Buildah, Skopeo and Podman
excerpt: Buildah, Skopeo and Podman
date: 2019-08-25 19:19:09
tags:
  - Container
categories: [Cloud, Container]
---

# Buildah, Skopeo and Podman

- [Buildah](https://github.com/containers/buildah) is a tool for building Open Container Initiative (OCI) container images
- [Skopeo](https://github.com/containers/skopeo) is a command line utility that performs various operations on container images and image repositories.
- Podman is a tool for managing Pods, Containers, and Container Images

## Buildah

The [Buildah](https://github.com/containers/buildah) package provides a command line tool that can be used to:

- create a working container, either from scratch or using an image as a starting point
- create an image, either from a working container or via the instructions in a Dockerfile
- images can be built in either the OCI image format or the traditional upstream docker image format
- mount a working container's root filesystem for manipulation
- unmount a working container's root filesystem
- use the updated contents of a container's root filesystem as a filesystem layer to create a new image
- delete a working container or an image
- rename a local container

## Skopeo
[Skopeo](https://github.com/containers/skopeo) is a command line utility that performs various operations on container images and image repositories.

[Skopeo](https://github.com/containers/skopeo) can work with OCI images as well as the original Docker v2 images.

[Skopeo](https://github.com/containers/skopeo) works with API V2 registries such as Docker registries, the Atomic registry, private registries, local directories and local OCI-layout directories. Skopeo does not require a daemon to be running to perform these operations which consist of:

- Copying an image from and to various storage mechanisms. For example you can copy images from one registry to another, without requiring privilege.
- Inspecting a remote image showing its properties including its layers, without requiring you to pull the image to the host.
- Deleting an image from an image repository.
- When required by the repository, skopeo can pass the appropriate credentials and certificates for authentication

## Podman

At a high level, the scope of libpod and [Podman](https://github.com/containers/libpod) is the following:
- Support multiple image formats including the OCI and Docker image formats.
- Support for multiple means to download images including trust & image verification.
- Container image management (managing image layers, overlay filesystems, etc).
- Full management of container lifecycle.
- Support for pods to manage groups of containers together.
- Resource isolation of containers and pods.
- Support for a Docker-compatible CLI interface through Podman.
- Integration with CRI-O to share containers and backend code.
