---
title: Build BusyBox in Docker
description: Build BusyBox in Docker
date: 2020-04-12 21:01:31
tags:
  - Programming
  - C/C++
categories: [Programming, C/C++]
permalink: build-busybox-in-docker
---

# Build BusyBox in Docker

## Download BusyBox

```bash
# aria2c https://busybox.net/downloads/busybox-1.31.1.tar.bz2

06/09 13:43:20 [NOTICE] Downloading 1 item(s)

06/09 13:43:21 [NOTICE] Download complete: /tmp/tmp.e6xObfyoNJ/busybox-1.31.1.tar.bz2

Download Results:
gid   |stat|avg speed  |path/URI
======+====+===========+=======================================================
201313|OK  |    12MiB/s|/tmp/tmp.e6xObfyoNJ/busybox-1.31.1.tar.bz2

Status Legend:
(OK):download completed.
```

## Build BusyBox by Fedora

### Create the Container

```bash
# docker run --rm -it -v `pwd`:/root -w /tmp fedora:31
```

### Setup repository mirrors

Only if you are happy with the my repository settings of [Fedora](fedora.repo).

```bash
# rm -f /etc/yum.repos.d/*
# cp /root/fedora.repo /etc/yum.repos.d/
```

### Install packages

```bash
# dnf upgrade --setopt=install_weak_deps=False --best --assumeyes
# dnf install --setopt=install_weak_deps=False --best --assumeyes \
    bzip2 which findutils diffutils file procps-ng make gcc
```

### Build BusyBox

```bash
# tar -xjf /root/busybox-1.31.1.tar.bz2
# cd busybox-1.31.1/

# make defconfig
# make -j4
```

### Verification

```bash
# du -ks busybox busybox_unstripped
944     busybox
1108    busybox_unstripped

# file busybox busybox_unstripped
busybox:            ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=17eedba515201d32d51f6d010dab4058562cd300, for GNU/Linux 3.2.0, stripped
busybox_unstripped: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=17eedba515201d32d51f6d010dab4058562cd300, for GNU/Linux 3.2.0, not stripped

# ./busybox
```

## Build BusyBox by Ubuntu or Debian

### Create the Container

For Ubuntu:
```bash
# docker run --rm -it -v `pwd`:/root -w /tmp ubuntu:18.04
```

For Debian:
```bash
# docker run --rm -it -v `pwd`:/root -w /tmp debian:10
```

### Setup repository mirrors

Only if you are happy with the my repository settings of [Ubuntu](sources-1804.list) or [Debian](sources-10.list).

For Ubuntu:
```bash
# cp /root/sources-1804.list /etc/apt/sources.list
```

For Debian:
```bash
# cp /root/sources-10.list /etc/apt/sources.list
```

### Install packages

```bash
# apt-get update && apt-get dist-upgrade -y
# apt-get install -y --no-install-recommends \
    bzip2 findutils diffutils file procps make gcc libc6-dev
```

### Build BusyBox

```bash
# tar -xjf /root/busybox-1.31.1.tar.bz2
# cd busybox-1.31.1/

# make defconfig
# make -j4
```

### Verification

For Ubuntu:
```bash
# du -ks busybox busybox_unstripped
1024    busybox
1192    busybox_unstripped

# file busybox busybox_unstripped
busybox:            ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=716c4b5f55a8fa42cc3d2935320ec949a25a628d, stripped
busybox_unstripped: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=716c4b5f55a8fa42cc3d2935320ec949a25a628d, not stripped

# ./busybox
```

For Debian:
```bash
# du -ks busybox busybox_unstripped
984     busybox
1148    busybox_unstripped

# file busybox busybox_unstripped
busybox:            ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=af33517e2bf0576ee99f201ef211b3cf1a1c4d28, stripped
busybox_unstripped: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=af33517e2bf0576ee99f201ef211b3cf1a1c4d28, not stripped

# ./busybox
```
