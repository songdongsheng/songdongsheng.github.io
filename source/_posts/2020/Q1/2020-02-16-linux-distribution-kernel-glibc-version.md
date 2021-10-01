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

but if your static linking program use `libdl` and the GNU C Library version in the build system is newer than the version in the target system, you may get the following error:

    dl-call-libc-early-init.c:37: _dl_call_libc_early_init: Assertion `sym != NULL' failed.

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
SUSE 15                    | 2018-07        | 2.26, **2.31 (SP3)**  | 4.12, **5.3 (SP2)**, **5.14 (SP4)**
ubuntu:18.04               | 2018-04        | 2.27                  | 4.15
RHEL/CentOS 8              | 2019-05        | 2.28                  | 4.18
Debian 10                  | 2019-07        | 2.28                  | 4.19
ubuntu:20.04               | 2020-04        | **2.31**              | **5.4**
Debian 11                  | 2021-08        | 2.31                  | 5.10
fedora:35                  | 2021-11        | 2.34                  | 5.14
openEuler 22.03 LTS        | 2022-04        | 2.34                  | 5.10
ubuntu:22.04               | 2022-04        | 2.34                  | 5.15
fedora:36                  | 2022-05        | 2.35                  | 5.16
RHEL 9                     | 2022-05        | 2.34                  | 5.14
alpine:3.14                | 2021-06        | 1.2.2  - musl         | 5.10
alpine:3.15                | 2021-11        | 1.2.2  - musl         | 5.15
alpine:3.16                | 2022-05        | 1.2.3  - musl         | 5.15

## docker images

```bash
# shopt -s checkwinsize
# echo "Lines: $LINES, Columns: $(tput cols)"
# echo "Lines: $LINES, Columns: $COLUMNS"
Lines: 30, Columns: 96

# podman images | grep '<none>' | awk '{print $3}' | xargs --no-run-if-empty podman rmi
# podman images | tail +2 | sort -V -k1,2 | while read IMG_PATH IMG_TAG REST; do date;echo podman pull $IMG_PATH:$IMG_TAG; podman pull $IMG_PATH:$IMG_TAG; echo; done
# podman images | tail +2 | sort -V -k1,2
REPOSITORY                               TAG         IMAGE ID      CREATED       SIZE
docker.io/library/alpine                 3.16        e66264b98777  2 days ago    5.82 MB
docker.io/library/debian                 11          c4905f2a4f97  2 weeks ago   129 MB
docker.io/library/debian                 testing     32bab12525f7  2 weeks ago   124 MB
docker.io/library/ubuntu                 20.04       53df61775e88  3 weeks ago   75.1 MB
docker.io/library/ubuntu                 22.04       d2e4e1f51132  3 weeks ago   80.3 MB
docker.io/openeuler/openeuler            22.03-lts   81d2a1304c8d  5 weeks ago   221 MB
quay.io/fedora/fedora                    36          3a66698e6040  2 weeks ago   169 MB
registry.access.redhat.com/ubi8          latest      1264065f6ae8  3 weeks ago   225 MB
registry.access.redhat.com/ubi8-micro    latest      35b6dc4ceb9d  3 weeks ago   29.4 MB
registry.access.redhat.com/ubi8-minimal  latest      08c1631d50a3  3 weeks ago   94.8 MB
registry.access.redhat.com/ubi9          latest      46720ac964ac  3 weeks ago   229 MB
registry.access.redhat.com/ubi9-micro    latest      52122e9e5c45  3 weeks ago   26.2 MB
registry.access.redhat.com/ubi9-minimal  latest      8b9dbc6a9765  3 weeks ago   129 MB
registry.suse.com/suse/sle15             latest      af0af42a133b  28 hours ago  119 MB

```
