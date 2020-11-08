---
title: Linux Kernel and GNU C Library version of common Linux distributions
description: Linux Kernel and GNU C Library version of common Linux distributions
date: 2020-02-16 17:08:09
tags:
  - Linux
categories: [Operating system, Linux]
permalink: linux-distribution-kernel-glibc-version
---

# Linux Kernel and GNU C Library version of common Linux distributions

## statically linking

statically linking the specified libraries:

    -Wl,-Bstatic -lluajit-5.1 -lssl -lcrypto -lsctp -Wl,-Bdynamic

## glibc versions

If we link with the lowest version of the C Library, then we can achieve maximum compatibility.

Linux distribution version | GNU C Library version | Linux Kernel version
---------------------------|-----------------------|---------------------
RHEL/CentOS 5              | 2.5                   | 2.6.18
ubuntu:10.04               | 2.11.1                | 2.6.32
Debian 6                   | 2.11.3                | 2.6.32
RHEL/CentOS 6              | 2.12                  | 2.6.32
Debian 7                   | 2.13                  | 3.2
ubuntu:12.04               | 2.15                  | 3.2
RHEL/CentOS 7              | 2.17                  | 3.10
ubuntu:14.04               | 2.19                  | 3.13
Debian 8                   | 2.19                  | 3.16
SUSE 12                    | 2.22                  | 3.12 ~ 4.12
ubuntu:16.04               | 2.23                  | 4.4
Debian 9                   | 2.24                  | 4.9
SUSE 15                    | 2.26                  | 4.12 ~ 5.3
ubuntu:18.04               | 2.27                  | 4.15
RHEL/CentOS 8              | 2.28                  | 4.18
Debian 10                  | 2.28                  | 4.19
ubuntu:20.04               | 2.30                  | 5.4
fedora:33                  | 2.32                  | 5.8

## docker images

```bash
# docker images | tail +2 | sort -V -k1,2
alpine                                3.8                 c8bccc0af957        3 weeks ago         4.41MB
alpine                                3.9                 82f67be598eb        3 weeks ago         5.53MB
alpine                                3.10                af341ccd2df8        3 weeks ago         5.56MB
alpine                                3.11                e7d92cdc71fe        4 weeks ago         5.59MB
centos                                6                   d0957ffdf8a2        11 months ago       194MB
centos                                7                   5e35e350aded        3 months ago        203MB
centos                                8                   470671670cac        4 weeks ago         237MB
debian                                8                   955c8f8160c2        2 weeks ago         129MB
debian                                9                   92416e205014        2 weeks ago         101MB
debian                                10                  a8797652cfd9        2 weeks ago         114MB
registry.access.redhat.com/rhel6      latest              06a6be77b9ea        4 weeks ago         200MB
registry.access.redhat.com/rhel7      latest              2ef6ad3f3825        3 weeks ago         205MB
registry.access.redhat.com/ubi7/ubi   latest              74954afdc5e7        3 weeks ago         205MB
registry.access.redhat.com/ubi8/ubi   latest              fd73e6738a95        3 weeks ago         231MB
registry.suse.com/suse/sle15          15.1                574954199ba6        2 weeks ago         114MB
registry.suse.com/suse/sles12sp5      latest              98d7a32c3a4f        45 hours ago        99.3MB
ubuntu                                14.04               6e4f1fe62ff1        2 months ago        197MB
ubuntu                                16.04               96da9143fb18        5 weeks ago         124MB
ubuntu                                18.04               ccc6e87d482b        5 weeks ago         64.2MB
ubuntu                                20.04               2a4d239ad3cc        5 weeks ago         73.4MB
```
