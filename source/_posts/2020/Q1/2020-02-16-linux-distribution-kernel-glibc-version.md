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

Linux distribution         | Release date   | GNU C Library | GCC support library | Linux Kernel
---------------------------|----------------|---------------|---------------------|---------------------
RHEL/CentOS 5              | 2007-04        | 2.5           |                     | 2.6.18
Ubuntu 10.04               | 2010-04        | 2.11.1        |                     | 2.6.32
Debian 6                   | 2011-02        | 2.11.3        |                     | 2.6.32
RHEL/CentOS 6              | 2010-11        | 2.12          | vsyscall=emulate    | 2.6.32
Debian 7                   | 2013-05        | 2.13          | vsyscall=emulate    | 3.2
Ubuntu 12.04               | 2012-04        | 2.15          | 4.6.3               | 3.2
***RHEL/CentOS 7***        | **2014-06**    | **2.17**      | 4.8.5               | 3.10
Ubuntu 14.04               | 2014-04        | 2.19          | 4.9.3               | 3.13
SLE 12                     | 2014-10        | 2.19 -> 2.22  | 4.8.3 -> 11.3       | [3.12 -> 4.12](https://www.suse.com/lifecycle/)
Debian 8                   | 2015-04        | 2.19          | 4.9.2               | 3.16
Ubuntu 16.04               | 2016-04        | 2.23          | 6.0                 | 4.4
Debian 9                   | 2017-06        | 2.24          | 6.3                 | 4.9
SLE 15                     | 2018-07        | 2.26 -> 2.31  | 7.3.1 -> 11.3       | [4.12 -> 5.14](https://www.suse.com/lifecycle/)
Ubuntu 18.04               | 2018-04        | 2.27          | 8.4                 | 4.15
***RHEL/CentOS/Alma 8***   | **2019-05**    | **2.28**      | 8.5                 | 4.18
Debian 10                  | 2019-07        | 2.28          | 8.3                 | 4.19
***openEuler 20.03***      | **2020-03**    | **2.28**      | **7.3**             | 4.19
Ubuntu 20.04               | 2020-04        | 2.31          | 10.3                | 5.4
SLE 15 SP3                 | 2021-06        | 2.31          | 11.2, 11.3 (SP4)    | 5.3, 5.14 (SP4)
***Debian 11***            | **2021-08**    | **2.31**      | **10.2**            | 5.10
Fedora 35                  | 2021-11        | 2.34          | 11.3                | 5.14
***openEuler 22.03***      | **2022-03**    | **2.34**      | **10.3**            | 5.10
***RHEL/CentOS/Alma 9***   | **2022-05**    | **2.34**      | 11.2                | 5.14
Ubuntu 22.04               | 2022-04        | 2.35          | 12.0                | 5.15
Fedora 36                  | 2022-05        | 2.35          | 12.1                | 5.16
***openEuler 22.09***      | 2022-09        | 2.35          | **10.3**            | 5.10
***Debian 12***            | 2023-06 ???    | [2.36](https://tracker.debian.org/pkg/glibc) | [12.2](https://gcc.gnu.org/develop.html) | [6.0 ???](https://tracker.debian.org/pkg/linux)
[SUSE Adaptable Linux Platform (ALP)](https://download.opensuse.org/repositories/SUSE:/ALP/) | ????-?? | 2.36 | 12.1 ??? | [5.19 ???](https://download.opensuse.org/repositories/SUSE:/ALP/standard/x86_64/)
Alpine 3.16                | 2022-05        | musl [1.2.3](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.16-stable/main/musl/APKBUILD) | libgcc 11.2 | [5.15](https://gitlab.alpinelinux.org/alpine/aports/-/blob/3.16-stable/main/linux-lts/APKBUILD)

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
docker.io/library/almalinux              8           4580d9e4bab7  7 weeks ago   195 MB
docker.io/library/almalinux              9           fceff10c5236  7 weeks ago   195 MB
docker.io/library/alpine                 3.16        e66264b98777  2 days ago    5.82 MB
docker.io/library/debian                 10          12d6c5111e34  4 days ago    119 MB
docker.io/library/debian                 11          c4905f2a4f97  2 weeks ago   129 MB
docker.io/library/debian                 testing     32bab12525f7  2 weeks ago   124 MB
docker.io/library/ubuntu                 20.04       53df61775e88  3 weeks ago   75.1 MB
docker.io/library/ubuntu                 22.04       d2e4e1f51132  3 weeks ago   80.3 MB
docker.io/openeuler/openeuler            22.03       81d2a1304c8d  5 weeks ago   221 MB
quay.io/centos/centos                    stream8     3dc89df4c17e  3 weeks ago   419 MB
quay.io/centos/centos                    stream9     16b647e23524  2 weeks ago   156 MB
quay.io/fedora/fedora                    36          3a66698e6040  2 weeks ago   169 MB
quay.io/fedora/fedora                    37          fb1b20c07506  7 hours ago   190 MB
registry.access.redhat.com/ubi8          latest      1264065f6ae8  3 weeks ago   225 MB
registry.access.redhat.com/ubi8-micro    latest      35b6dc4ceb9d  3 weeks ago   29.4 MB
registry.access.redhat.com/ubi8-minimal  latest      08c1631d50a3  3 weeks ago   94.8 MB
registry.access.redhat.com/ubi9          latest      46720ac964ac  3 weeks ago   229 MB
registry.access.redhat.com/ubi9-micro    latest      52122e9e5c45  3 weeks ago   26.2 MB
registry.access.redhat.com/ubi9-minimal  latest      8b9dbc6a9765  3 weeks ago   129 MB
registry.suse.com/bci/bci-base           15.3        15d798578180  25 hours ago  122 MB
registry.suse.com/bci/bci-base           15.4        400c37fc52ea  27 hours ago  125 MB
```
