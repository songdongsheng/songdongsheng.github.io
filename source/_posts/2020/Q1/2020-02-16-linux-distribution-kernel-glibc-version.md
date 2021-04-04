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

Linux distribution version | Release date   | GNU C Library version | Linux Kernel version
---------------------------|----------------|-----------------------|---------------------
RHEL/CentOS 5              | 2007-04        | 2.5                   | 2.6.18
ubuntu:10.04               | 2010-04        | 2.11.1                | 2.6.32
Debian 6                   | 2011-02        | 2.11.3                | 2.6.32
RHEL/CentOS 6              | 2010-11        | 2.12                  | 2.6.32
Debian 7                   | 2013-05        | 2.13                  | 3.2
ubuntu:12.04               | 2012-04        | 2.15                  | 3.2
RHEL/CentOS 7              | 2014-06        | 2.17                  | 3.10
ubuntu:14.04               | 2014-04        | 2.19                  | 3.13
Debian 8                   | 2015-04        | 2.19                  | 3.16
SUSE 12                    | 2014-10        | 2.22                  | 3.12, 4.4, 4.12
ubuntu:16.04               | 2016-04        | 2.23                  | 4.4
Debian 9                   | 2017-06        | 2.24                  | 4.9
SUSE 15                    | 2018-07        | 2.26, 2.31 (SP3)      | 4.12, 5.3 (SP2)
ubuntu:18.04               | 2018-04        | 2.27                  | 4.15
RHEL/CentOS 8              | 2019-05        | 2.28                  | 4.18
Debian 10                  | 2019-07        | 2.28                  | 4.19
ubuntu:20.04               | 2020-04        | 2.30                  | 5.4
Debian 11                  | 2021-08        | 2.31                  | 5.10
fedora:34                  | 2021-04        | 2.33                  | 5.11
alpine:3.12                | 2020-05        | 1.1.24 - musl         | 5.4
alpine:3.13                | 2021-01        | 1.2.2  - musl         | 5.10
alpine:3.14                | 2021-06        | 1.2.2  - musl         | 5.10

## docker images

```bash
# docker images | grep '<none>' | awk '{print $3}' | xargs --no-run-if-empty docker rmi
# docker images | tail +2 | sort -V -k1,2 | while read IMG_PATH IMG_TAG REST; do date;echo docker pull $IMG_PATH:$IMG_TAG; docker pull $IMG_PATH:$IMG_TAG; echo; done
# docker images | tail +2 | sort -V -k1,2
alpine                                3.12      13621d1b12d4   3 months ago   5.58MB
alpine                                3.13      6dbb9cc54074   3 months ago   5.61MB
alpine                                3.14      021b3423115f   22 hours ago   5.6MB
busybox                               glibc     f4d471eafcba   2 months ago   5.21MB
busybox                               musl      9ad2c435a887   2 months ago   1.43MB
busybox                               uclibc    69593048aa3a   2 months ago   1.24MB
debian                                9         2c3ad12c6ecf   2 weeks ago    101MB
debian                                10        0980b84bde89   2 weeks ago    114MB
fedora                                34        dce66322d647   2 weeks ago    178MB
registry.access.redhat.com/ubi8/ubi   latest    0ced1c7c9b23   2 weeks ago    226MB
registry.suse.com/suse/sle15          latest    4d0dbe92b78b   5 days ago     114MB
ubuntu                                18.04     39a8cfeef173   11 days ago    63.1MB
ubuntu                                20.04     1318b700e415   11 days ago    72.8MB
```
