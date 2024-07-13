---
title: Linux Kernel and GNU C Library version of common Linux distributions
excerpt: Linux Kernel and GNU C Library version of common Linux distributions
date: 2020-02-16 17:08:09
tags:
  - Linux
categories: [Operating system, Linux]
---

# Linux Kernel and GNU C Library version of common Linux distributions

## statically linking

statically linking the specified libraries:

    -Wl,-Bstatic -lluajit-5.1 -lssl -lcrypto -lsctp -Wl,-Bdynamic

but if your static linking program use `libdl` and the GNU C Library version in the build system is newer than the version in the target system, you may get the following error:

    # Using 'dlopen' in statically linked applications requires at runtime the shared libraries from the glibc version used for linking
    dl-call-libc-early-init.c:37: _dl_call_libc_early_init: Assertion `sym != NULL' failed.

## glibc versions

If we link with the lowest version of the C Library, then we can achieve maximum compatibility.

Linux distribution      | Release date  | GNU C Library |GCC support library| Linux Kernel
------------------------|---------------|---------------|-------------------|--------------
Ubuntu 10.04            | 2010-04       | 2.11.1        | 4.4.3             | 2.6.32
Debian 6                | 2011-02       | 2.11.3        | 4.4.5             | 2.6.32
RHEL 6                  | 2010-11       | 2.12          | 4.4.7             | 2.6.32
Debian 7                | 2013-05       | 2.13          | 4.7.2             | 3.2
Ubuntu 12.04            | 2012-04       | 2.15          | 4.6.3             | 3.2
**RHEL 7**              | 2014-06       | **2.17**      | 4.8.5             | 3.10
Ubuntu 14.04            | 2014-04       | 2.19          | 4.9.3             | 3.13
SLE 12                  | 2014-10       | 2.19 -> 2.22  | 4.8.3 -> 13.2     | [3.12 -> 4.12](https://www.suse.com/lifecycle/)
Debian 8                | 2015-04       | 2.19          | 4.9.2             | 3.16
Ubuntu 16.04            | 2016-04       | 2.23          | 4.9.3             | 4.4
Debian 9                | 2017-06       | 2.24          | 6.3               | 4.9
Amazon Linux 2 LTS      | 2018-06       | 2.26          | 7.3               | 4.14
SLE 15                  | 2018-07       | 2.26 -> 2.31  | 7.3 -> 13.2       | [4.12 -> 5.14](https://www.suse.com/lifecycle/)
**Ubuntu 18.04**        | 2018-04       | **2.27**      | 8.4               | 4.15
RHEL 8                  | 2019-05       | 2.28          | 8.5               | 4.18
Debian 10               | 2019-07       | 2.28          | 8.3               | 4.19
openEuler 20.03         | 2020-03       | 2.28          | 7.3               | 4.19
Ubuntu 20.04            | 2020-04       | 2.31          | 10.3              | 5.4
**Debian 11**           | 2021-08       | **2.31**      | 10.2              | 5.10
openEuler 22.03         | 2022-03       | 2.34          | 10.3              | 5.10
**RHEL 9**              | 2022-05       | **2.34**      | 11.4              | 5.14
Amazon Linux 2023       | 2023-03       | 2.34          | 11.4              | 6.1
CBL-Mariner 2.0         | 2022-05       | 2.35          | 11.2              | 5.15
Ubuntu 22.04            | 2022-04       | 2.35          | 12.1              | 5.15
Debian 12               | 2023-06       | [2.36](https://tracker.debian.org/pkg/glibc) | [12.2](https://packages.debian.org/bookworm/libgcc-s1) | [6.1](https://tracker.debian.org/pkg/linux)
openEuler 24.04         | 2024-06       | 2.38          | 12.3              | 6.6
SLE 15 SP6              | 2024-06       | 2.38          | 13.3              | 6.4
**Ubuntu 24.04**        | 2024-04       | **2.39**      | 14.0              | 6.8
Fedora 40               | 2024-04       | 2.39          | 14.1              | 6.9
Debian 13               | ***2025-06*** | [2.40 ?](https://tracker.debian.org/pkg/glibc) | [14.2 ?](https://packages.debian.org/trixie/libgcc-s1) | [6.12 ?](https://tracker.debian.org/pkg/linux)
RHEL 10                 | ***2025-08*** | [2.40 ?](https://composes.stream.centos.org/stream-10/production/latest-CentOS-Stream/compose/BaseOS/x86_64/os/Packages/) | [14.2 ?](https://composes.stream.centos.org/stream-10/production/latest-CentOS-Stream/compose/BaseOS/x86_64/os/Packages/) | [6.12 ?](https://composes.stream.centos.org/stream-10/production/latest-CentOS-Stream/compose/BaseOS/x86_64/os/Packages/)
[SUSE Adaptable Linux Platform (ALP)](https://download.opensuse.org/repositories/SUSE:/ALP/) | ***2025-??*** | 2.39 ? | 14.0 ? | 6.8 ?
[**openSUSE Tumbleweed**](https://download.opensuse.org/tumbleweed/repo/oss/x86_64/) | ***Rolling*** | 2.39 ? | 14.1 ? | 6.9 ?
Alpine 3.18             | [2023-05](https://alpinelinux.org/releases/) | musl [1.2.4](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.18-stable/main/musl/APKBUILD) | [libgcc 12.2](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.18-stable/main/gcc/APKBUILD) | [6.1](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.18-stable/main/linux-lts/APKBUILD)
Alpine 3.19             | [2023-12](https://alpinelinux.org/releases/) | musl [1.2.4](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.19-stable/main/musl/APKBUILD) | [libgcc 13.2](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.19-stable/main/gcc/APKBUILD) | [6.6](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.19-stable/main/linux-lts/APKBUILD)
Alpine 3.20             | [2024-05](https://alpinelinux.org/releases/) | musl [1.2.5](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.20-stable/main/musl/APKBUILD) | [libgcc 13.2](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.20-stable/main/gcc/APKBUILD) | [6.6](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.20-stable/main/linux-lts/APKBUILD)
Alpine edge             | [***2024-11***](https://alpinelinux.org/releases/) | musl [1.2.5](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/main/musl/APKBUILD) | [libgcc 13.2](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/main/gcc/APKBUILD) | [6.6](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/main/linux-lts/APKBUILD)

## libgcc versions

The following GCC versions add new functions to **libgcc** ([`libgcc/libgcc-std.ver.in`](https://gcc.gnu.org/git/?p=gcc.git;a=blob;f=libgcc/libgcc-std.ver.in;hb=HEAD) and [`libgcc/config/i386/libgcc-glibc.ver`](https://gcc.gnu.org/git/?p=gcc.git;a=blob;f=libgcc/config/i386/libgcc-glibc.ver;hb=HEAD))：

+ **GCC 14.0.0**
+ **GCC 13.0.0**
+ **GCC 12.0.0**
+ **GCC 7.0.0**
+ **GCC 4.8.0**
+ ... (Too old to need to list anymore)

## docker images

```bash
# shopt -s checkwinsize
# echo "Lines: $(tput lines), Columns: $(tput cols)"
# echo "Lines: $LINES, Columns: $COLUMNS"
Lines: 30, Columns: 96

# podman images | grep '<none>' | awk '{print $3}' | xargs --no-run-if-empty podman rmi
# podman images | tail +2 | sort -V -k1,2 | while read IMG_PATH IMG_TAG REST; do date;echo podman pull $IMG_PATH:$IMG_TAG; podman pull $IMG_PATH:$IMG_TAG; echo; done
# podman images | tail +2 | sort -V -k1,2
REPOSITORY                                          TAG         IMAGE ID      CREATED       SIZE
docker.io/library/almalinux                         8           acaca326f3b3  8 weeks ago   196 MB
docker.io/library/almalinux                         9           b0b3f56026dd  8 weeks ago   193 MB
docker.io/library/alpine                            3.17        042a816809aa  2 weeks ago   7.34 MB
docker.io/library/debian                            11          5c8936e57a38  2 weeks ago   129 MB
docker.io/library/debian                            testing     556061af5f11  2 weeks ago   121 MB
docker.io/library/ubuntu                            20.04       d5447fc01ae6  7 weeks ago   75.2 MB
docker.io/library/ubuntu                            22.04       6b7dfa7e8fdb  7 weeks ago   80.3 MB
docker.io/songdongsheng/openeuler                   22.03       e2c2b88bb007  6 days ago    160 MB
ghcr.io/oracle/oraclelinux                          7           258c049720a7  17 hours ago  271 MB
ghcr.io/oracle/oraclelinux                          8           15d3782e65b8  17 hours ago  237 MB
ghcr.io/oracle/oraclelinux                          9           6d4f5c87c123  17 hours ago  234 MB
mcr.microsoft.com/cbl-mariner/base/core             2.0         4ec990a6bc0d  39 hours ago  68.4 MB
public.ecr.aws/amazonlinux/amazonlinux              2           20b4c80b1755  4 days ago    172 MB
public.ecr.aws/amazonlinux/amazonlinux              2023        eef352c7a904  3 days ago    149 MB
quay.io/fedora/fedora                               37          19c0ae4dd222  7 weeks ago   190 MB
quay.io/fedora/fedora                               38          c9bfca6d0ac2  8 days ago    196 MB
registry.access.redhat.com/ubi7                     latest      f0c1470d8cb2  11 days ago   217 MB
registry.access.redhat.com/ubi7-minimal             latest      0f3e1be52c0b  11 days ago   84.8 MB
registry.access.redhat.com/ubi8                     latest      6a2ef33ab97f  3 weeks ago   214 MB
registry.access.redhat.com/ubi8-micro               latest      03a08867970f  2 weeks ago   28.5 MB
registry.access.redhat.com/ubi8-minimal             latest      35585f3ca6c6  3 weeks ago   94.5 MB
registry.access.redhat.com/ubi8/openjdk-11          latest      4383be399b12  10 days ago   397 MB
registry.access.redhat.com/ubi8/openjdk-11-runtime  latest      d526bdf5e61d  10 days ago   358 MB
registry.access.redhat.com/ubi8/openjdk-17          latest      59fd01ab51ad  10 days ago   408 MB
registry.access.redhat.com/ubi8/openjdk-17-runtime  latest      42cb53376c53  10 days ago   366 MB
registry.access.redhat.com/ubi9                     latest      ed8d4815d368  11 days ago   219 MB
registry.access.redhat.com/ubi9-micro               latest      fc923451d5b9  11 days ago   26.1 MB
registry.access.redhat.com/ubi9-minimal             latest      5b3c7c785802  11 days ago   97.4 MB
registry.access.redhat.com/ubi9/openjdk-11          latest      cac6dcc81e20  10 days ago   393 MB
registry.access.redhat.com/ubi9/openjdk-11-runtime  latest      bde7a6c813fb  10 days ago   359 MB
registry.access.redhat.com/ubi9/openjdk-17          latest      f3368e202a72  10 days ago   404 MB
registry.access.redhat.com/ubi9/openjdk-17-runtime  latest      aeb5671874f4  10 days ago   366 MB
registry.opensuse.org/opensuse/leap                 15.4        ce2ffde521dc  2 weeks ago   116 MB
registry.opensuse.org/opensuse/tumbleweed           latest      c2a77db6cf5d  33 hours ago  106 MB
registry.suse.com/bci/bci-base                      15.4        a357b1e79e7e  43 hours ago  122 MB
registry.suse.com/bci/bci-busybox                   15.4        6a88b3a3c425  2 weeks ago   14.5 MB
registry.suse.com/bci/bci-micro                     15.4        07d9e5464374  2 weeks ago   25.8 MB
registry.suse.com/bci/bci-minimal                   15.4        ac521a188b44  28 hours ago  48.1 MB
registry.suse.com/bci/openjdk                       latest      0f14e1b28472  43 hours ago  334 MB
registry.suse.com/bci/openjdk-devel                 latest      68a33af805cf  22 hours ago  402 MB
registry.suse.com/suse/sle15                        15.4        a357b1e79e7e  43 hours ago  122 MB
registry.suse.com/suse/sles12sp5                    latest      9e344f88a8fc  25 hours ago  99.5 MB
```
