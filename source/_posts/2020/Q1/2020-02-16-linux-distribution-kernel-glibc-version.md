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
[**<font color="#C73A3A">RHEL 7</font>**](https://access.redhat.com/support/policy/updates/errata/#Life_Cycle_Dates) | 2014-06 | **2.17** | **4.8.5** | 3.10
[**<font color="#C73A3A">AnolisOS 7</font>**](https://openanolis.cn/anolisos/7) | [2022-02](https://docs.openanolis.cn/document/detail/ojobfl8g) | **2.17** | **4.8.5** | 4.19
Ubuntu 14.04            | 2014-04       | 2.19          | 4.9.3             | 3.13
Debian 8                | 2015-04       | 2.19          | 4.9.2             | 3.16
[**<font color="#C73A3A">SLES 12 SP5</font>**](https://www.suse.com/lifecycle/) | 2019-12 | 2.22 | 13.3      | 4.12
Ubuntu 16.04            | 2016-04       | 2.23          | 5.4.0             | 4.4
Debian 9                | 2017-06       | 2.24          | 6.3               | 4.9
Amazon Linux 2 LTS      | 2018-06       | 2.26          | 7.3               | 4.14
Ubuntu 18.04            | 2018-04       | 2.27          | 8.4               | 4.15
[**<font color="#E67E22">RHEL 8</font>**](https://access.redhat.com/support/policy/updates/errata/#Life_Cycle_Dates) | 2019-05 | **2.28** | **8.5** | 4.18
<font color="#C73A3A">Debian 10</font> | 2019-07       | 2.28          | 8.3               | 4.19
openEuler 20.03         | 2020-03       | 2.28          | 7.3               | 4.19
[OpenCloudOS 8.10](https://docs.opencloudos.org/en/release/oc_intro/) | 2024-10 | [2.28](https://mirrors.opencloudos.tech/opencloudos/8/BaseOS/x86_64/os/Packages/) | 8.5 | 5.4
[**<font color="#2F77B3">AnolisOS 8.10</font>**](https://openanolis.cn/anolisos/8) | [2025-04](https://docs.openanolis.cn/document/detail/ojobfl8g) | [**2.28**](http://mirrors.openanolis.cn/anolis/8/BaseOS/x86_64/os/Packages/) | 8.5 | 5.10
<font color="#C73A3A">**Ubuntu 20.04**</font> | [2020-04](https://ubuntu.com/about/release-cycle) | 2.31          | 10.5              | 5.4
<font color="#E67E22">Debian 11</font> | 2021-08       | 2.31          | 10.2              | 5.10
openEuler 22.03         | 2022-03       | 2.34          | 10.3              | 5.10
[**<font color="#2F77B3">RHEL 9</font>**](https://access.redhat.com/support/policy/updates/errata/#Life_Cycle_Dates) | 2022-05 | 2.34 | 11.5 | 5.14
Amazon Linux 2023       | 2023-03       | 2.34          | 11.4              | 6.1
CBL-Mariner 2.0         | 2022-05       | 2.35          | 11.2              | 5.15
<font color="#2F77B3">Ubuntu 22.04</font> | [2022-04](https://ubuntu.com/about/release-cycle) | 2.35          | 12.3              | 5.15
[<font color="#2F77B3">Debian 12</font>](https://wiki.debian.org/DebianBookworm) | [2023-06](https://www.debian.org/releases/) | [2.36](https://tracker.debian.org/pkg/glibc) | [12.2](https://packages.debian.org/bookworm/libgcc-s1) | [6.1](https://tracker.debian.org/pkg/linux)
[<font color="#2E9E5B">**OpenCloudOS 9.4**</font>](https://docs.opencloudos.org/en/release/oc_intro/)     | 2025-05       | **[2.38](https://mirrors.opencloudos.tech/opencloudos/9/BaseOS/x86_64/os/Packages/)** | **12.3** | 6.6
[AnolisOS 23.3](https://openanolis.cn/anolisos/23) | [2025-06](https://docs.openanolis.cn/document/detail/ojobfl8g) | [2.38](http://mirrors.openanolis.cn/anolis/23/updates/x86_64/os/Packages/) | 12.3 | 6.6
openEuler 24.03         | 2024-06       | 2.38          | 12.3              | 6.6
[**<font color="#2F77B3">SLES 15 SP7</font>**](https://www.suse.com/lifecycle/) | 2025-06 | 2.38 | 14.3      | [6.4](https://www.suse.com/support/kb/doc/?id=000019587#SLE15SP7)
**<font color="#2E9E5B">Ubuntu 24.04</font>** | [2024-04](https://ubuntu.com/about/release-cycle) | 2.39          | 14.0              | 6.8
[**<font color="#2E9E5B">RHEL 10</font>**](https://access.redhat.com/support/policy/updates/errata/#Life_Cycle_Dates) | 2025-05 | 2.39 | 14.2 | 6.12
[***<font color="#7A52CC">CentOS Stream 10</font>***](https://mirror.stream.centos.org/10-stream/BaseOS/x86_64/os/Packages/) | ***Rolling*** | 2.39 | ***14.3?*** | 6.12
[**<font color="#2E9E5B">SLES 16.0</font>**](https://www.suse.com/lifecycle/#product-suse-linux-enterprise-server) | **2025-11** | [**2.40**](https://download.opensuse.org/distribution/leap/16.0/repo/oss/aarch64/) | [**15.1**](https://download.opensuse.org/distribution/leap/16.0/repo/oss/x86_64/) | [**6.12**](https://www.suse.com/support/kb/doc/?id=000019587)
[**<font color="#2E9E5B">Debian 13</font>**](https://wiki.debian.org/DebianTrixie) | 2025-08 | [2.41](https://packages.debian.org/trixie/libc6) | [14.2](https://packages.debian.org/trixie/libgcc-s1) | [6.12](https://packages.debian.org/trixie/linux-libc-dev)
Fedora 42               | 2025-04       | 2.41          | 15.0              | 6.14
[Ubuntu 25.10](https://wiki.ubuntu.com/QuestingQuokka) | [***2025-10***](https://wiki.ubuntu.com/Releases) | [***2.42***](https://packages.ubuntu.com/questing/libc6) | [***15.2***](https://packages.ubuntu.com/questing/libgcc-s1) | [***6.17***](https://packages.ubuntu.com/questing/linux-libc-dev)
Fedora 43               | 2025-10       | 2.42          | 15.2              | 6.17
[***<font color="#7A52CC">Debian 14</font>***](https://wiki.debian.org/DebianForky) | [***2027-08?***](https://www.debian.org/releases/) | [***2.46?***](https://tracker.debian.org/pkg/glibc) | [***16.2?***](https://tracker.debian.org/pkg/libstdc++6) | [***7.6?***](https://tracker.debian.org/pkg/linux)
[**<font color="#7A52CC">openSUSE Tumbleweed</font>**](https://download.opensuse.org/tumbleweed/repo/oss/x86_64/) | ***Rolling*** | ***2.42?*** | ***15.2?*** | ***6.17?***
Alpine 3.21             | [2024-12](https://alpinelinux.org/releases/) | [musl 1.2.5](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.21-stable/main/musl/APKBUILD) | [libgcc 14.2](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.21-stable/main/gcc/APKBUILD) | [6.12](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.21-stable/main/linux-lts/APKBUILD)
**<font color="#2E9E5B">Alpine 3.22</font>** | [2025-05](https://alpinelinux.org/releases/) | [**musl 1.2.5**](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.22-stable/main/musl/APKBUILD) | [**libgcc 14.2**](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.22-stable/main/gcc/APKBUILD) | [**6.12**](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.22-stable/main/linux-lts/APKBUILD)
**<font color="#7A52CC">Alpine edge</font>** | [***2025-12?***](https://alpinelinux.org/releases/) | musl [1.2.5?](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/main/musl/APKBUILD) | [libgcc 15.2?](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/main/gcc/APKBUILD) | [6.12?](https://gitlab.alpinelinux.org/alpine/aports/-/blob/master/main/linux-lts/APKBUILD)


## libgcc & libstdc++ versions

- [The GNU C/C++ Library ABI Policy and Guidelines](https://gcc.gnu.org/onlinedocs/libstdc++/manual/abi.html)

The following GCC versions add new functions to **libgcc** ([`libgcc/libgcc-std.ver.in`](https://gcc.gnu.org/git/?p=gcc.git;a=blob;f=libgcc/libgcc-std.ver.in;hb=HEAD) and [`libgcc/config/i386/libgcc-glibc.ver`](https://gcc.gnu.org/git/?p=gcc.git;a=blob;f=libgcc/config/i386/libgcc-glibc.ver;hb=HEAD)) & **libstdc++** ([`libstdc++-v3/config/abi/pre/gnu.ver`](https://github.com/gcc-mirror/gcc/blob/master/libstdc++-v3/config/abi/pre/gnu.ver)):

+ **GCC 16.1.0** (Only `libstdc++.so.6` has the symbol version `GLIBCXX_3.4.35` added)
+ **GCC 15.1.0** (Only `libstdc++.so.6` has the symbol version `GLIBCXX_3.4.34` added)
+ **GCC 14.1.0** (Both `libgcc_s.so.1` and `libstdc++.so.6` have symbol versions added)
+ **GCC 13.1.0**
+ **GCC 12.1.0**
+ **GCC 7.1.0**
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
docker.io/songdongsheng/anolisos                    7           856d891a4d7d  2 days ago    181 MB
docker.io/songdongsheng/anolisos                    8           b39daaa5a422  2 days ago    129 MB
docker.io/songdongsheng/openeuler                   22.03       e2c2b88bb007  6 days ago    160 MB
docker.io/songdongsheng/openeuler                   24.03       fcd7328d3b91  3 months ago  167 MB
docker.io/songdongsheng/opencloudos                 8           43f03fc1c7af  2 weeks ago   142 MB
docker.io/songdongsheng/opencloudos                 9           8d77637ceb8c  3 weeks ago   153 MB
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
